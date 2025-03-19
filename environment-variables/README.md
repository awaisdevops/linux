# Understanding Environment Variables in Linux!

This README provides an overview of environment variables in operating systems, particularly focusing on Linux, and their importance for user configurations, application management, and secure data handling.

## Table of Contents

- [User Environment](#user-environment)
- [Environment Variables](#environment-variables)
  - [Viewing Environment Variables](#viewing-environment-variables)
  - [Creating Custom Environment Variables](#creating-custom-environment-variables)
  - [Using Environment Variables in Scripts](#using-environment-variables-in-scripts)
- [Persisting Environment Variables](#persisting-environment-variables)
  - [Making Environment Variables Permanent](#making-environment-variables-permanent)
- [Global Environment Variables](#global-environment-variables)
- [Example Use Case](#example-use-case)
- [Commands Summary](#commands-summary)

## User Environment

Each user in a multi-user system (Linux, Windows, MacOS) has their own environment, which can be customized. Users can configure their preferences, such as the default shell, font size, color themes, and default applications like web browsers or editors. This ensures that changes made by one user do not affect others.

## Environment Variables

Environment variables are key-value pairs that store user-specific settings and preferences. They are dynamic and can be changed by users, unlike fixed configurations.

For example:
- **`HOME`**: Points to the user's home directory.
- **`SHELL`**: Points to the default shell program (e.g., `/bin/bash`).
- **`EDITOR`**: Specifies the default text editor.

### Viewing Environment Variables

You can view the current environment variables using the `printenv` command:

```bash
printenv
```

To display the environment variables using `less` for easier scrolling:

```bash
printenv | less
```

### Creating Custom Environment Variables

You can create environment variables to store values such as database credentials or other sensitive data.

For example, to create an environment variable for a database username:

```bash
export DB_USERNAME=my_database_user
```

You can check the value of an environment variable:

```bash
echo $DB_USERNAME
```

To modify an environment variable:

```bash
export DB_NAME=new_db_name
```

To remove an environment variable:

```bash
unset DB_USERNAME
```

### Using Environment Variables in Scripts

Environment variables can be accessed in scripts by referencing them with the `$` symbol:

Example script (`example.sh`):

```bash
#!/bin/bash
echo "Database User: $DB_USERNAME"
```

## Persisting Environment Variables

By default, environment variables set with `export` are only available for the current session. Once you close the terminal, these variables are lost.

### Making Environment Variables Permanent

To make environment variables permanent, add them to your shell’s configuration file (e.g., `.bashrc` for Bash, `.zshrc` for Z shell). After editing the configuration file, use the `source` command to apply the changes immediately.

For example, to add an environment variable permanently to `.bashrc`:

1. Open `.bashrc` in your text editor:

   ```bash
   nano ~/.bashrc
   ```

2. Add the following line at the end of the file:

   ```bash
   export DB_USERNAME=my_database_user
   ```

3. Apply the changes by sourcing the file:

   ```bash
   source ~/.bashrc
   ```

Now, the environment variable will be available in all future terminal sessions.

## Global Environment Variables

Some environment variables are global and available to all users on the system. For example, the `PATH` variable contains directories where executable files are stored. This allows the system to locate commands like `ls` or `python` without needing their full paths.

You can view the `PATH` variable by running:

```bash
echo $PATH
```

### Modifying the Global `PATH` Variable

To add custom directories to the global `PATH`, you can modify system-wide files like `/etc/environment`. However, if you only want to change the `PATH` for your user, you can add it to your `.bashrc`.

For example, to add a directory to your user-specific `PATH`:

```bash
export PATH=$PATH:/home/user/scripts
```

Then apply the changes:

```bash
source ~/.bashrc
```

## Example Use Case

Let’s create a simple script and add its directory to `PATH` so that it can be run from anywhere on the system.

### Step 1: Create a Simple Script

Create a script called `welcome`:

```bash
echo -e "#!/bin/bash\n\necho 'Welcome to my Linux guide! User: $USER'" > ~/scripts/welcome
chmod +x ~/scripts/welcome
```

### Step 2: Add the Script Directory to `PATH`

To make this script accessible from anywhere, add the script’s directory to `PATH` by editing `.bashrc`:

```bash
export PATH=$PATH:/home/user/scripts
```

### Step 3: Apply the Changes

```bash
source ~/.bashrc
```

Now, you can run the script from anywhere:

```bash
welcome
```

## Commands Summary

Here’s a summary of important commands related to environment variables:

- View all environment variables:
  ```bash
  printenv
  ```

- View environment variables with `less`:
  ```bash
  printenv | less
  ```

- Create an environment variable:
  ```bash
  export MY_VAR=value
  ```

- Access an environment variable:
  ```bash
  echo $MY_VAR
  ```

- Change the value of an environment variable:
  ```bash
  export MY_VAR=new_value
  ```

- Remove an environment variable:
  ```bash
  unset MY_VAR
  ```

- Make environment variables permanent (edit `.bashrc`):
  ```bash
  nano ~/.bashrc
  ```

- Apply changes from `.bashrc`:
  ```bash
  source ~/.bashrc
  ```

- View the `PATH` variable:
  ```bash
  echo $PATH
  ```

## Conclusion

Environment variables are essential for managing user configurations, securing sensitive data, and enabling application flexibility across different environments. By using commands like `export`, `echo`, and `source`, you can efficiently configure and manage your environment.
