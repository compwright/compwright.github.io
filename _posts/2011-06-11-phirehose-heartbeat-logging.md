---
id: 283
title: Heartbeat logging while consuming Twitter streams using Phirehose
date: 2011-06-11T10:15:49+00:00
author: Jonathon
layout: post
guid: http://jonathonhill.net/?p=283
permalink: /2011-06-11/phirehose-heartbeat-logging/
categories:
  - Notebook
tags:
  - API
  - Phirehose
  - PHP
  - Twitter
---
<a href="http://github.com/fennb/phirehose" target="_blank">Phirehose</a> is an awesomely useful <a href="http://dev.twitter.com/pages/streaming_api" target="_blank">Twitter Streaming API</a> client library, written in PHP by <a href="http://github.com/fennb" target="_blank">Fenn Bailey</a>.

Heartbeat logging is something that I originally added for <a href="http://rainmakerapp.com" target="_blank">Rainmaker</a>, and I finally got around to contributing those modifications, which you can see here on <a href="https://github.com/fennb/phirehose/commit/7118ca973f67998a72a303497f6f6a6ced1099b8" target="_blank">GitHub</a>.

## Why log heartbeats in Phirehose?

  * To gain assurance that Phirehose is still alive, and actually functioning. In our case, missing tweets means lost money and unhappy clients. We needed to monitor this very closely.
  * To enable automatically detecting connection drops and rewinding the count parameter to pick up those tweets, or backfilling them in using the Twitter Search API.
  * To collect usage data for reporting purposes.

## Usage

To use this, simply declare a `heartbeat(array $data)` method in your Phirehose child class.<!--more-->

Here is an example:

<pre>&lt;?php

// USAGE:
//
//   require_once 'phirehose.php';
//   $dbh = mysql_connect($host, $username, $password);
//   PhirehoseConsumer::Initialize($dbh);
//   PhirehoseConsumer::start();

class PhirehoseConsumer extends Phirehose {

	private static $instance;
	private static $dbh;

	protected $consumer_start_time = 0;
	protected $consumer_uptime = 0;

	public static function Initialize($dbh) {
		if (! self::$instance instanceof self) {
			self::$dbh = $dbh;
			self::$instance = new self ( TWITTER_API_USERNAME, TWITTER_API_PASSWORD, Phirehose::METHOD_FILTER );
		}
	}

	protected static function get_last_heartbeat() {
		$sql = 'SELECT time_stamp FROM stream_log ORDER BY time_stamp DESC LIMIT 1';
		$result = mysql_query ( self::$dbh, $sql );
		$last_heartbeat_ts = mysql_result ( $result, 0, 'time_stamp' );
		if (strtotime ( $last_heartbeat_ts ) === FALSE)
			$last_heartbeat_ts = date ( 'Y-m-d H:i:s' ); // default to now
		return $last_heartbeat_ts;
	}

	protected static function get_missed_count() {

		// get timestamp of the most recent intake script heartbeat
		$last_heartbeat_ts = self::get_last_heartbeat ();

		// how long since we were last running?
		$downtime = ( int ) (time () - strtotime ( $last_heartbeat_ts ));

		// Based on the past average, how many tweets did we probably miss?
		//
		// This calculation is based on the statusRate averaged over a period of
		// time not greater than $downtime, and multiplied by a 1.5x fudge factor.
		$sql = &lt;&lt;&lt;SQL
SELECT CEIL(1.5 * AVG(statusRate) * $downtime) AS count
FROM stream_log WHERE
    (UNIX_TIMESTAMP('$last_heartbeat_ts') - UNIX_TIMESTAMP(time_stamp) &gt; 0) AND
    (UNIX_TIMESTAMP('$last_heartbeat_ts') - UNIX_TIMESTAMP(time_stamp) &gt; 0) &lt;= $downtime
SQL;
		$result = mysql_query ( self::$dbh, $sql );
		$count = mysql_result ( $result, 0, 'count' );

		return ($count === FALSE) ? 0 : ( int ) $count;
	}

	public static function start() {
		// get timestamp of the most recent heartbeat
		$last_heartbeat_ts = self::get_last_heartbeat ();

		// Estimate how many tweets were missed, and rewind.
		//
		// Note that if you're using the filter streaming method, this will fail with the following error:
		//   "The Streaming API count parameter is not allowed in role statusDefaultFiltered."
		$count = self::get_missed_count ();
		if ($count) {
			self::$instance-&gt;log ( "Using count: $count" );
			self::$instance-&gt;setCount ( $count );
		}

		// run backfill script in background - uses Twitter Search API
		self::$instance-&gt;log ( "Backfilling from $last_heartbeat_ts" );
		passthru ( "backfill.php '$last_heartbeat_ts' &gt;/dev/null &" );

		self::$instance-&gt;checkFilterPredicates ();
		self::$instance-&gt;consume ();
	}

	public function enqueueStatus($status) {
		// do something with your tweet
		return TRUE;
	}

	public function heartbeat(array $data) {
		$sql = &lt;&lt;&lt;SQL
INSERT INTO stream_log SET
    time_stamp = CURRENT_TIMESTAMP,
    elapsed = $data[elapsed],
    statusRate = $data[statusRate],
    statusCount = $data[statusCount],
    enqueueSpent = $data[enqueueSpent],
    enqueueSpentAvg = $data[enqueueSpentAvg],
    filterCheckCount = $data[filterCheckCount],
    filterCheckSpent = $data[filterCheckSpent],
    idlePeriod = $data[idlePeriod],
    maxIdlePeriod = $data[maxIdlePeriod]
SQL;
		mysql_query ( $dbh, $sql );
	}

	protected function checkFilterPredicates() {
		// useful for debugging
		$this-&gt;consumer_uptime = time () - $this-&gt;consumer_start_time;
		$this-&gt;log ( sprintf ( 'Uptime: %s, memory usage: %skb', $this-&gt;consumer_uptime, number_format ( memory_get_usage () / 1024, 0 ) ) );

		// update filter predicates...
	}

	protected function connect() {
		$this-&gt;consumer_start_time = time ();
		parent::connect ();
	}

}</pre>