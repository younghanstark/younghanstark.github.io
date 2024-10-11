---
layout: blog
title: "Shell Script Programming using zx"
date: 2022-11-30 00:00
---

## Shell Script Programming w/ No Tool
- Doesn't support OOP
- Doesn't support structures like JSON
- Doesn't support string or array manipulation methods
- Often has to call separate Python or Node.js scripts

### Shell Script Programming Scenarios
- Build processes
- CI/CD
- Computer maintenance

## Shell Script Programming w/ child_process module
To deal with the problems above, we can do shell script programming in node.js with using child_process module.

### child_process
- Node.js built-in module
- Provides the ability to spawn subprocesses

### Example: ls -lh /usr
```javascript
const { spawn } = require('node:child_process');
const ls = spawn('ls', ['-lh', '/usr']);

ls.stdout.on('data', (data) => {
 console.log(`stdout: ${data}`);
});
```

But this method is not developer-friendly.

## Shell Script Programming w/ zx
> Bash is great, but when it comes to writing more complex scripts, many people prefer a more convenient programming language. JavaScript is a perfect choice, but the Node.js standard library requires additional hassle before using. The zx package provides useful wrappers around child_process, escapes arguments and gives sensible defaults.

### How to use zx
- `npm i -g zx` to install
- Write your scripts in a file with an .mjs extension
- Add `#!/usr/bin/env zx` to the beginning of your zx script
- Run you script with `zx ./script.mjs`

### Example: ls -lh /usr
```javascript
#!/usr/bin/env zx

const data = await $`ls -lh /usr`
console.log(`stdout: ${data}`)
```

### Example: backup repositories from github
```javascript
#!/usr/bin/env zx

const username = await question('What is your GitHub username? ')
const token = await question('Do you have GitHub token in env? ', {
 choices: Object.keys(process.env),
})

let headers = {}
if (process.env[token]) {
 headers = {
   Authorization: `token ${process.env[token]}`,
 }
}

let res = await fetch(
 `https://api.github.com/users/${username}/repos?per_page=1000`,
 { headers }
)
const data = await res.json()
const urls = data.map((x) =>
 x.git_url.replace('git://github.com/', 'git@github.com:')
)

await $`mkdir -p backups`
cd('./backups')

for (const url of urls) {
 await $`git clone ${url}`
}
```

## Reference

- [https://github.com/google/zx](https://github.com/google/zx)