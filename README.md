# Projeme Hoşgeldiniz Aşağıdaki adımları takip ederek projeyi localinizde çalıştırabilirsiniz İyi çalışmalar.
# "https://github.com/Furkan-Alay/GitopsProjem" ve "https://github.com/Furkan-Alay/GitappProjem" buradaki resource kısmına gidip ayrı ayrı her ikisi için fork işlemini yapmanız gerekmektedir.
# Daha sonra terminalinizi açıp SSH key oluşturmanız gerekmektedir. buradaki kodları sırayla terminale yazınız. 
* cd
* cd ~/.ssh
* ssh-keygen =>>> Burada private ve public ssh key oluşacaktır.
* cat [sshkeyadınız] =>>> Burada içeriği kopyalayın.
# Daha sonra oluşturduğunuz public Key bilgilerinizi Kendi Github SSH Key kısmına kaydedin. (Github hesabınıza girin >>>> Settings >>>>> SSH and GPG keys >>> New SSH key >>> Gerekli bilgileri girin ve Add basın) 
# Terminalinizde Git yükle değilse git paketini yükleyiniz.
# Daha sonra Terminalinizi açın
* mkdir ~/Desktop/[Klasör adınız]
* cd ~/Desktop/[Klasör adınız]
* Oluşturduğunuz fork kaynak kodlarına girin ve SSH linklerini kopyalayın.
* git clone [SSH Klone linki(Terraform kaynak kodu)] 
* git clone [SSH Klone linki(Uygulama kaynak kodu)]
## Bu projede Terraform kaynak kodu için "iac-vprofile" fork ve Uygulama kodu için "vprofile-action" isimleri kullandığımızı varsayacağım başka bir isim kullandıysanız değiştirin.
* ls
* cd iac-vprofile
* git config core.sshCommand "ssh -i ~/.ssh/gitaction -F /dev/null"
* cd ../vprofile-action/
* git config core.sshCommand "ssh -i ~/.ssh/gitaction -F /dev/null"
## Burada kendi Github hesap adınızı ve mailinizi giriniz.
* git config --global user.name devops541
* git config --global user.email furkanalay428@gmail.com
* cd ..
#### Yeni bir klasör kopyaladık istediğiniz ismi burada verebilirsiniz.
* cp -r iac-vprofile/ main-iac
* cd iac-vprofile
* git checkout stage
### Steps
* terraform init
* terraform fmt -check
* terraform validate
* terraform plan -out planfile
* terraform apply -auto-approve -input=false -parallelism=1 planfile
####
