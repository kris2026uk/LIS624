# Koha

The last step to build our library website is to install and link up Koha to our other programs.

The first step is to create a new VM that will be able to run this program. It requires more memory than our initial VM has. 

We need to make it the same system of before, only larger.
Ubuntu 24.04 lts minimal 86/64 at 20 gb

fire wall

updates

download edit
wget https://github.com/microsoft/edit/releases/download/v1.2.1/edit-1.2.0-x86_64-linux-gnu.tar.zst
tar -xf  edit-1.2.0-x86_64-linux-gnu.tar.zst
sudo install -m 0755 edit /usr/local/bin/edit
