# Intro

  Fedora Silverblue is an amazing operating system whose workflows are
  all centered around the immutable host system and the user's toolbox
  containers. As I have found it difficult to solve some of the
  issues I first encountered when dealing with it, I am going to
  document the best solutions I have devised for them so far.

# Flatpaks

  Flatpak programs work well(?) when used standalone. If you want them
  to interact with other programs, you are better installing them in
  your home dir or inside a container. For instance, I am currently
  making usage of no flatpak programs.

# Containers

  At first, I have decided to create a toolbox container named `main`
  in which I install the packages I need which are available directly
  from Fedora repositories.

  Whenever I felt the need to install an external `package.rpm`, I
  took advantage of the container-centric workflows and sandboxed it
  in the following way:
  1. first, I create a container called `package` with `toolbox create
     package`
  2. them, I enter the newly created container with `toolbox enter package`
  3. finally, I install the package with `sudo dnf install
     package.rpm`

# Running containerized programs from outside

  In order to make it easy to execute a program which is installed
  inside a container, I came up with this simple script which I call
  `run-inside`.

  ```
  #!/bin/sh
  toolbox run --container $1 bash -c "$2"
  ```

  This way, it would be possible to run `emacs` which is installed
  inside the `main` container with `run-inside main emacs`. This
  script ensures the execued program inherits the environment
  variables set in `.bashrc`. I have found this to be useful since I
  have some toolchains installed directly in home, such as `rustup`
  and `node`.

  Analogous scripts can be devised for the command line interpreter of
  your choice.

# The hellish VSCode installation

  Gosh, that was no simple job. I am documenting it so I am not
  chasing instructions around the internet again.

  1. create a container for vscode with `toolbox create vscode`
  2. enter the container with `toolbox enter vscode`
  3. `sudo dnf install` these implicit dependencies: `libxshmfence`,
     `libX11-xcb`, `google-droid-sans-fonts`
  4. get the official VSCode rpm package from its site and `sudo dnf
     install` it
  5. Now you shuld be able to call `code` from within the container
     and `run-inside vscode code` from outside
