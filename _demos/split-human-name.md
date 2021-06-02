---
title: Human name splitter demo
permalink: /demos/split-human-name
layout: post
---
Split full names into first and last names using [split-human-name](https://npmjs.com/package/split-human-name)

- Splits a name into exactly two fields `{ firstName, lastName }`
- Fixes UPPERCASE, lowercase, iNVERSE CASE, and otherwise FUnkY cAse
- Handles couples ("John and Jane Doe")
- Gracefully degrades to put the entire string in `firstName` if there are multiple last names

<form action="https://us-central1-demos-220218.cloudfunctions.net/split-human-name-dev-demo" method="POST">
    <p>Enter full names, one per line:</p>
    <p><textarea name="names" required></textarea></p>
    <p><button type="submit" class="button">Submit</button></p>
</form>
