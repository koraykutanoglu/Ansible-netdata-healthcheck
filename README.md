# Ansible-netdata-healthcheck

#Ansible İle netdata kurulumu ve healthcheck bilgi toplamak

##**Nedir?**

- Ansible ile bağlı olduğumuz makinalardan aktif olarak veri çekeceğiz
- İlgili makinalara netdata kurup healthcheck ile veri çekeceğiz

##**Gereklilikler**

- Ubuntu 20.04 LTS
- Centos 7.2


###**Uygulama**

- Bu iki makineyi VMware Üzerinde kurabilirsiniz. Ben DigitalOcean üzerinden oluşturuyorum

####**Centos üzerinde yapılacak işlem**

- python3 kurulu olmalıdır 

```
yum install python3
```

####**Ubuntu üzerindeki işlemler**

- Sistemi güncelliyoruz

```
apt-get update && apt-get upgrade
```

- **htop**, monitoring aracıdır.
- **python3-pip**, paket yönetim sistemidir. Pythonda yazılmış paketleri indirmek için kullanılır
- **sshpass**, şifre sorulmadan sunucuya bağlanmanın bir çözümüdür 

```
apt install htop python3-pip sshpass -y
```

- Ansibleyi indirelim

```
apt install ansible
```

- Karşı sunucuya bağlanabilmek için ssh-key oluşturuyoruz
- **-t**, anahtar tipini belirlemekte kullanılır
- **b**, kullanılmak istenen bit sayısını belirtir
- Kodu girdikten sonra işlem tamamlancaya kadar entere basın

```
ssh-keygen -t rsa -b 4096 
```

- Oluşturulan key karşı sunucuya kopyalanacak
- **root**, kullanıcı adınız şeklinde düzenleyiniz
- **192.168.17.135**, IP adresiniz şeklinde düzenleyiniz 


```
ssh-copy-id root@192.168.17.135
```

- Ansbile ile bağlantı sağlıyabilmek için hosts dosyamızı düzenliyoruz

```
nano /etc/ansible/hosts
```


- Aşağıdakileri yapıştırın;

```
[test]

165.232.69.119
165.232.78.102

[test:vars]
ansible_python_interpreter=/usr/bin/python3
ansible_user=root
ansible_password=b>*+_~<(4m?F}&Kn
```


####**Bağlantı testi**

- Sunuculara ping atıyoruz

```
ansible all -m ping
```

- Çıktı Şöyle olmalıdır:

```
165.232.69.119 | SUCCESS => {
    "ansible_facts": {
         "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
165.232.78.102 | SUCCESS => {
    "ansible_facts": {
         "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
```



###**Bilgi Toplama Araçlarının Kurulması**


- Dosyaların kurulacağı alana gidelim

```
cd /etc/ansible
```

- DOsyaları çekelim


```
git clone https://github.com/koraykutanoglu/Ansible-netdata-healthcheck
```

- Öncelikle Ansible-netdata-healthcheck dosyasının içerisindeki dosyaları /etc/ansible dizinine çıkaralım 

```
mv Ansible-netdata-healthcheck/centos7-healthcheck /etc/ansible && mv Ansible-netdata-healthcheck/netdata-install /etc/ansible
```

- Şimdi sunuculara netdata kuralım 

```
cd netdata-install && ansible-playbook netdata.yml -l all -u root
```

- netdata kontrol

```
ip-adresi:19999
```

- Sistem bilgilerini alalım


```
cd .. && cd centos7-healthcheck && ansible-playbook healthcheck-final.yml -l all -u root 
```

- Aşağıdaki komutla report adında bir klasörün oluşup oluşmadığına bakın

```
ls
```

- report klasörüne girdiğinizde csv formatında bilgileri göreceksiniz

```
cd report && ls
```

- csv formatını json formatına değiştirin ve çıktı:


```
0	
************************************	"System Health Status"
1	
************************************	"************************************"
2	
************************************	"Hostname : centos-s-1vcpu-1gb-fra1-01"
3	
************************************	"Operating System : CentOS Linux 7 (Core)"
4	
************************************	"Kernel Version : 3.10.0-957.27.2.el7.x86_64"
5	
************************************	"OS Architecture : 64 Bit OS"
6	
************************************	"System Uptime : up by 4:50 hours"
7	
************************************	"Current System Date & Time : Thu Aug 27 21:32:54 2020"
8	
************************************	"Checking For Read-only File System[s]"
9	
************************************	"-------------------------------------"
10	
************************************	".....No read-only file system[s] found."
11	
************************************	"Checking For Currently Mounted File System[s]"
12	
************************************	"--------------------------------------------------------------------------"
13	
************************************	"/dev/vda1  on  /  type  xfs  (rw"
14	
************************************	"Checking For Disk Usage On Mounted File System[s]"
15	
************************************	"--------------------------------------------------------------------------"
16	
************************************	"( 0-85% = OK/HEALTHY"
17	
************************************	"--------------------------------------------------------------------------"
18	
************************************	"Mounted File System[s] Utilization (Percentage Used):"
19	
************************************	"/dev/vda1  /  5%  \u001b[47;32m   ------  OK/HEALTHY  \u001b[0m"
20	
************************************	"Checking For Zombie Processes"
21	
************************************	"-------------------------------------"
22	
************************************	"No zombie processes found on the system."
23	
************************************	"Checking For INode Usage"
24	
************************************	"--------------------------------------------------------------------------"
25	
************************************	"( 0-85% = OK/HEALTHY"
26	
************************************	"--------------------------------------------------------------------------"
27	
************************************	"INode Utilization (Percentage Used):"
28	
************************************	"/dev/vda1  /  1%  \u001b[47;32m   ------  OK/HEALTHY  \u001b[0m"
29	
************************************	"Checking SWAP Details"
30	
************************************	"-------------------------------------"
31	
************************************	"Total Swap Memory in MiB : 0"
32	
************************************	"Swap Free Memory in MiB : 0"
33	
************************************	"Checking For Processor Utilization"
34	
************************************	"-------------------------------------"
35	
************************************	"Current Processor Utilization Summary :"
36	
************************************	"Checking For Load Average"
37	
************************************	"-------------------------------------"
38	
************************************	"Current Load Average : 0.16"
39	
************************************	"Most Recent 3 Reboot Events"
40	
************************************	"--------------------------------------------------------------------------"
41	
************************************	"reboot   system boot  3.10.0-957.27.2. Thu Aug 27 16:42 - 21:32  (04:49)"
42	
************************************	"Most Recent 3 Shutdown Events"
43	
************************************	"--------------------------------------------------------------------------"
44	
************************************	"No shutdown events are recorded."
45	
************************************	"Top 5 Memory Resource Hog Processes"
46	
************************************	"--------------------------------------------------------------------------"
47	
************************************	"%MEM   PID  PPID USER     STAT COMMAND"
48	
************************************	"2.5 15473     1 netdata  SNsl /opt/netdata/bin/srv/netdata -P /opt/netdata/var/run/netdata/netdata.pid -D"
49	
************************************	"2.1 15638 15473 netdata  SNl  /usr/bin/python /opt/netdata/usr/libexec/netdata/plugins.d/python.d.plugin 1"
50	
************************************	"2.0 15640 15473 netdata  SNl  /opt/netdata/usr/libexec/netdata/plugins.d/go.d.plugin 1"
51	
************************************	"1.7 16777 15638 netdata  RN   /usr/bin/python /opt/netdata/usr/libexec/netdata/plugins.d/python.d.plugin 1"
52	
************************************	"1.6   867     1 root     Ssl  /usr/bin/python2 -Es /usr/sbin/tuned -l -P"
53	
************************************	"Top 5 CPU Resource Hog Processes"
54	
************************************	"--------------------------------------------------------------------------"
55	
************************************	"%CPU   PID  PPID USER     STAT COMMAND"
56	
************************************	"30.0 16716 16707 root     S+   /usr/bin/python3 /root/.ansible/tmp/ansible-tmp-1598563973.3520575-89670117736365/AnsiballZ_command.py"
57	
************************************	"4.8 16530  1144 root     Ss   sshd: root@pts/0"
58	
************************************	"0.8 15637 15473 root     RN   /opt/netdata/usr/libexec/netdata/plugins.d/apps.plugin 1"
59	
************************************	"0.5 15473     1 netdata  SNsl /opt/netdata/bin/srv/netdata -P /opt/netdata/var/run/netdata/netdata.pid -D"
60	
************************************	"0.1 15638 15473 netdata  SNl  /usr/bin/python /opt/netdata/usr/libexec/netdata/plugins.d/python.d.plugin 1"
```





