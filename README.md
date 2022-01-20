# Ansible Role: Rbenv, manage your app's Ruby environment

An Ansible Role that installs [Rbenv](https://github.com/rbenv/rbenv), and [Ruby programming language](https://www.ruby-lang.org) on Linux.

## Requirements

None.

## Role Variables

Available variables are listed below, along with default values (see `defaults/main.yml`):

If no version is specified, the latest version will be installed. Available [Rbenv releases](https://github.com/rbenv/rbenv/releases), [Ruby-build releases](https://github.com/rbenv/ruby-build/releases).

    rbenv_version: 1.2.0
    ruby_build_version: 20211227

If no installation directory is specified, `$HOME/.rbenv` is used.

    rbenv_install_dir: $HOME/.rbenv

If the directory for downloading and extracting the release archive is not specified, `/tmp` is used.

    rbenv_download_dir: $HOME/Downloads

After installation, you can add `~/.rbenv/bin` to your `$PATH` for access to the `rbenv` command-line utility.

   * For **bash**:

     Ubuntu Desktop users should configure `~/.bashrc`:
     ```bash
     echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bashrc
     ```

     On other platforms, bash is usually configured via `~/.bash_profile`:
     ```bash
     echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bash_profile
     ```

   * For **Zsh**:
     ```zsh
     echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.zshrc
     ```

   * For **Fish shell**:
     ```fish
     set -Ux fish_user_paths $HOME/.rbenv/bin $fish_user_paths
     ```

Follow instructions from to [set up rbenv shell integration](https://github.com/rbenv/rbenv#how-rbenv-hooks-into-your-shell).
<br />You can use a separate Role to modify the relevant configuration file(s)  on remote host(s) using Ansible.

**NOTE:** By default, the base tasks of a role are skipped if the application is already installed in the desired
<br />directory and no version upgrade is required.
<br />&nbsp;&nbsp;&nbsp;&nbsp;If you want to force an update to the latest available release versions of Rbenv and Ruby-build, you can force
<br />base role tasks to run by defining the `update_apps` variable and adding `rbenv` or `all` to the list. For example:
``` bash
$ ansible-playbook main.yaml -e "update_apps=[rbenv]"
```

To install Ruby programming language using installed versions of Rbenv and Ruby-build, desired version should be
<br />explicitly specified.

    ruby_version: 3.1.0

<br />Only [releases](https://www.ruby-lang.org/en/downloads/releases) with version number ***X.Y.Z*** are currently supported.

    # Supported version examples:
    3.1.0
    v2.7.5

    # Unsupported version examples:
    3.1.0-preview1
    3.0.0-rc1

However, Role will still try to install Ruby, but the unsupported release version will be truncated to the supported one:
<br />`3.1.0-preview1 => 3.1.0`.

If you want uninstall an explicitly specified version of Ruby, you can override the default value (`install`) of this variable:

    ruby_action: uninstall

Any other values will be ignored.

## Dependencies

`make`.

## Example Playbook

```yaml
- hosts: all
  roles:
    - role: ansible-role-rbenv-ruby
      ruby_version: 3.1.0
```

## License

MIT
