# Using devkitPro on a RPM-based distro

Since devkitPro packages are distributed as pacman packages, you need pacman to install
them. But some RPM-based distros do not have pacman. With a bit of work, you can get
devkitPro's custom pacman (`dkp-pacman`) installed, if you follow the steps below.


## Setup

1. Download the [rpm package from the releases
   page](https://github.com/dkosmari/devkitpro-pacman-rpm/releases/latest).

2. Install the rpm:

       sudo rpm --install devkitpro-pacman-*.x86_64.rpm

3. Now set up dkp-pacman according to [devkitPro's
   instructions](https://devkitpro.org/wiki/devkitPro_pacman):

       sudo dkp-pacman-key --init
       sudo dkp-pacman-key --recv  BC26F752D25B92CE272E0F44F7FD5492264BB9D0 --keyserver keyserver.ubuntu.com
       sudo dkp-pacman-key --lsign BC26F752D25B92CE272E0F44F7FD5492264BB9D0
       sudo dkp-pacman -Syu

   This last command will install an update for `dkp-pacman`, overriding the files
   installed by the RPM package.

Now you're ready to start installing the devkitPro packages you want.


## Preparing your environment

Most homebrew build scripts expect the environment variable `DEVKITPRO` to be pointing to the root of
devkitPro (`/opt/devkitpro`); similarly for `DEVKITARM` and `DEVKITPPC`. This is my
preferred way to set it up, on a distro that uses systemd:

  1. Create the file `~/.config/environment.d/devkitpro.conf` with these lines:

         DEVKITPRO=/opt/devkitpro
         DEVKITARM=${DEVKITPRO}/devkitARM
         DEVKITPPC=${DEVKITPRO}/devkitPPC

  2. Log out and back in again. You can check that the variable was defined properly by
     running this in a terminal:
     
         printenv DEVKITPRO

  3. (Optional) If you want to bring the devkitPro tools into your PATH during a terminal
     session, you can install the `dkp-toolchain-vars` package, which will install
     various shell scripts into `/opt/devkitpro` that can be sourced.

     For instance, to set up a Wii U environment, you can do:
     
         source $DEVKITPRO/wiiuvars.sh

     Note that these will override variables like `CC`, `CXX`, `CPPFLAGS`, etc, making it
     very difficult to use your native compiler.

     For maximum convenience, you can add these aliases to your `~/.bash_aliases`:
     
         alias 3ds-env="source $DEVKITPRO/3dsvars.sh"
         alias cube-env="source $DEVKITPRO/cubevars.sh"
         alias nds-env="$DEVKITPRO/ndsvars.sh"
         alias switch-env="$DEVKITPRO/switchvars.sh"
         alias wii-env="$DEVKITPRO/wiivars.sh"
         alias wiiu-env="$DEVKITPRO/wiiuvars.sh"

     Now you can just run `wiiu-env` to activate the Wii U environment, or similarly for
     the other consoles.


## Cheat sheet for pacman

### Update installed packages

    sudo dkp-pacman -Syu

### List packages

    dkp-pacman -Sl

### Install a package

    sudo dkp-pacman -S package-name

### Uninstall a package

    sudo dkp-pacman -R package-name

### Showing details about a package before installing it

    dkp-pacman -Si package-name

### Listing files in an installed package

    dkp-pacman -Ql package-name

