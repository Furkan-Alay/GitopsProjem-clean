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
#### Şimdi ise Terraform kodlarımızın içerisine Girip VPC infrastructure ve EKS Cluster oluşturalım.
* Terraform kaynak kodumuzun "iac-vprofile" klasörünü açıyoruz. VSCode uygulamasıyla açmamız gerekiyor.
* terraform klasörü içerisindeki "variables.tf" dosyamızı açıyoruz. AWS hesabımızdaki region bilgisini ve terraform ile oluşturacağımız EKS Cluster adını "default" kısmında yazıyoruz.
* Aynı şekilde terraform klasörü içerisindeki "terraform.tf" dosyamızı açıyoruz. "backend "s3"" içerisindeki "bucket" kısmına AWS Hesabımızda oluşturduğumuz S3 bucket name kısmını giriyoruz. AWS region bilgilerimizi de "region" kısmına giriyoruz. "key" kısmına dokunmuyoruz.
* "vpc.tf" dosyamızı açıyoruz. "name" değerine EKS Cluster için belirlediğimiz ismi giriyoruz.
* VSCode Source Control paneline gelip Projemizi "Commit & Push" seçeneğiyle Github'a push etmeliyiz.
* Çıkan ekranda Commit mesajını yazıp "Save" seçeneğine basacağız.
#### Şimdi ise Terraform kodlarımızı Github Actions kısmında çalıştıralım.
* Terraform kaynak kodumuzun "iac-vprofie" klasörünü VScode ile açtık.
* ".github/workflows" adında bir klasör oluşturduk.Bu klasör içinde "terraform.yml" dosyası oluşturuyoruz.Bu dosya ile github actions içerisinde terraform kodlarımızı çalıştırmış olacağız.
* Aşağıdaki kodları "terraform.yml" dosyamıza yapıştıralım
* name: "Vprofile IAC"  
on:
  push:
    branches:
      - main
      - stage
    paths:  
      - terraform/**
  pull_request:
    branches:
      - main
    paths:
      - terraform/**

env:
 # Credentials for deployment to AWS
 AWS_ACCESS_KEY_ID: ${{ secrets.AWS_ACCESS_KEY_ID }}
 AWS_SECRET_ACCESS_KEY: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
 # S3 bucket for the Terraform state
 BUCKET_TF_STATE: ${{ secrets.BUCKET_TF_STATE}}
 AWS_REGION: us-east-2
 EKS_CLUSTER: vprofile-eks

jobs:
   terraform:
     name: "Apply terraform code changes"
     runs-on: ubuntu-latest
     defaults:
       run:
         shell: bash
         working-directory: ./terraform

     steps:
       - name: Checkout source code 
         uses: actions/checkout@v4

       - name: Setup Terraform with specified version on the runner
         uses: hashicorp/setup-terraform@v2
         with:
           terraform_version: 1.6.3

       - name: Terraform init
         id: init
         run: terraform init -backend-config="bucket=$BUCKET_TF_STATE"

       - name: Terraform format
         id: fmt
         run: terraform fmt -check

       - name: Terraform validate
         id: validate
         run: terraform validate

       - name: Terraform plan
         id: plan
         run: terraform plan -no-color -input=false -out planfile
         continue-on-error: true

       - name: Terraform plan status
         if: steps.plan.outcome == 'failure' 
         run: exit 1
* Yukarıda yapıştırdığım kod içerisindeki "AWS_REGION" kısmına S3 Bucket ve ECR bölgemizi ekliyoruz.
* VSCode Source Control kısmından Commit&Push seçeneğine tıklıyoruz ve Commit mesajını yazdıktan sonra Save basıyoruz.
* Github Actions kısmında Pipeline adımlarının gerçekleştiğini görmüş olacağız. 
#### Şimdi ise Terraform kodlarımızla VPC Altyapısını ve EKS Cluster yapısını oluşturalım
* 
