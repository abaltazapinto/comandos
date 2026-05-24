
# Garantir que o sitema tem acesso ao repositorio principal completo.
 
sudo add-apt-repository "deb http://archive.ubuntu.com/ubuntu noble main universe restricted multiverse"

# Saber se existe para o teu UBUNTU ou nao o programa

apt search pinta

find e copiar ficheiro neste caso codigo

cp "$(find ~/Documents/GitHub -path '*transcribe.py' | grep 'Aula_3\|Aula 3\|Aula 02' | head -n 1)" .

# Mauqina Virtuais

VBoxManage showvminfo "Ubuntu-Base-24.04" | grep -Ei "graphics|vram|3d"

VBoxManage modifyvm "Ubuntu-Base-24.04-CLEAN" --graphicscontroller vboxvga --vram 128 --accelerate-3d off

VBoxManage showvminfo "Ubuntu-Base-24.04-CLEAN" | grep -Ei "graohics|vram|3d"

# quando instalas Linux

Quando a pen arranca carrega F12, e dpois em cima de try INstall Ubbuntu carrega e

e define a forma de arrancar

ghes.disable=1 nomodeset pci=noaer

a linha toda
	linux /casper/vmlinuz --- quiet splash ghes.disable=1 nomodeset pci=noaer
