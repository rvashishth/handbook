<details>
  <summary>How to use interpret escape in echo command i.e. echo key value pair in multiple lines</summary>
  </br>

```bash
echo "app=google env=dev"
```

will print `app=google env=dev` in one line as output. To interpret the escape characters we can pass `-e` option in `echo` command

```bash
echo -e "app=google\nenv=dev"
```

will output

```bash
app=google
env=dev
```

[echo command examples](https://www.tecmint.com/echo-command-in-linux/)

</details>

### [Shell Parameter Expension](http://www.gnu.org/software/bash/manual/html_node/Shell-Parameter-Expansion.html)

Here if `paramter` is unset or null `defaultValue` will be assigned to `username`

```bash
username=${parameter:-defaultValue}
```

or use another paramter do as below, this is not mentioned in the GNU manuals

```bash
username=${parameter:-$anotherParam}
```

<details>
    <summary>Symlink  linking</summary>
  
`ls -s source_file target_file`

</details>

Print Env variables/ list env variables </br>
`$ printenv`
