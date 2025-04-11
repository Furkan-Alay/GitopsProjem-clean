# Projeme Hoşgeldiniz Aşağıdaki adımları takip ederek projeyi localinizde çalıştırabilirsiniz İyi çalışmalar.
# "https://github.com/Furkan-Alay/GitopsProjem" ve "https://github.com/Furkan-Alay/GitappProjem" buradaki resource kısmına gidip ayrı ayrı her ikisi için SSH linkinden fork işlemini yapmanız gerekmektedir.
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
#### Şimdi ise Github Secrets adımlarımızı oluşturalım. Burada AWS ortamında oluşturduğumuz AWS Access Key,Secret Key,S3 Bucket ve ECR Repository bilgilerini Github Secret kısmına kaydedeceğiz.
* Terraform ve Uygulama kodlarını fork etmiştik. Bu kaynak kodlarına giriyoruz ve "Secret and variables" kısmındaki "Actions" kısımlarına basıyoruz.
* AWS hesabımıza giriyoruz ve projenin sonuna kadar kullanacağımız "Region" seçiyoruz.
* "IAM" servisini açıp Create User kısmına basıyoruz. Bu kullanıcının "Administrator Access" yetkisine sahip olması gerekiyor çünkü AWS hesabında kaynak kullanacaktır.
* IAM User kullanıcısı oluşturuyoruz. Bu kullanıcı için "Create Access Key" kısmına basıp "Access Key" ve "Secret Key" oluşturuyoruz.
* Access Key ve Secret Key bilgilerini kopyalıyoruz.
* Terraform kaynak kodumuzun ve Uygulama kısmına Github Secret kısmına bu bilgileri kaydediyoruz.
* AWS Hesabımızda "S3 Bucket" oluşturuyoruz ve "S3 Bucket Name" kopyalıyoruz.
* S3 Bucket Name kısmını Terraform kaynak kodumuzun içerisinde olduğu Github Secret kısmına kaydediyoruz.Buraya kaydetmemizin sebebi S3 Bucket servisini Terraform içerisinde kullanacağız  
* AWS Hesabımızda ECR(Elastic Container Registry) içerisinde private bir repository oluşturuyoruz ve ECR URL'mizi kopyalıyoruz.
* ECR URL'mizi Uygulama kaynak kodumuzun içerisinde olduğu Github Secret kısmına kaydediyoruz. 
