# Sh and Bash tips and tricks

## `script_dir`

How to get the dir of the script:

Here is an easy pretty good solution. From https://www.ostricher.com/2014/10/the-right-way-to-get-the-directory-of-a-bash-script/

```sh
script_dir="$( cd "$( readlink -f "${BASH_SOURCE[0]}" )" && pwd )"

#script_dir="$( cd "$( dirname "${BASH_SOURCE[0]}" )" && pwd )"
```

However, readlink might not be available, so `dirname` is an alternative.

And here is something more comprehensive

```sh
get_script_dir () {
     SOURCE="${BASH_SOURCE[0]}"
     # While $SOURCE is a symlink, resolve it
     while [ -h "$SOURCE" ]; do
          DIR="$( cd -P "$( dirname "$SOURCE" )" && pwd )"
          SOURCE="$( readlink "$SOURCE" )"
          # If $SOURCE was a relative symlink (so no "/" as prefix, need to resolve it relative to the symlink base directory
          [[ $SOURCE != /* ]] && SOURCE="$DIR/$SOURCE"
     done
     DIR="$( cd -P "$( dirname "$SOURCE" )" && pwd )"
     echo "$DIR"
}
```

