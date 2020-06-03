# zadanie1-kubernetes
potrzebne trzy maszyny z Centos 7 minimal-install zaktiualizowane 

1. Instalacja zalerznosci na hoscie z którego bedzie wykonywany deploy
      pip3 install -r requirements.txt
2. hosty na których bedzie wykonywany deploy powinny być w dns i/lub w pliku hosts
3. ansible działa po kluczach wiec dla root powinnno byc logowanie na hosty bez hasła po kluczu
    ssh-key-gen ( utworzenie klucza dla root na hoscie z ktorego puszaczmy playbook)
    ssh-copy-id root@nazwa_noda_kubernetes
4. w katalogu inventort/mycluster znajduje sie plik inventory.ini 

   Zawartośc zbioru :
   
  [all]
 master.makieta.local ansible_host=10.10.10.3  # ip=10.3.0.1 etcd_member_name=etcd1
 node1.makieta.local  ansible_host=10.10.10.4  # ip=10.3.0.2 etcd_member_name=etcd2
 node2.makieta.local  ansible_host=10.10.10.5  # ip=10.3.0.3 etcd_member_name=etcd3


[kube-master]
 master.makieta.local

[etcd]
 master.makieta.local



[kube-node]
 node1.makieta.local
 node2.makieta.local

[calico-rr]

[k8s-cluster:children]
kube-master
kube-node
calico-rr
~          

w sekcji [all] wpisujemy nasze hosty ich nazwy i ip
w sekcji [etd] wpisujemy hosty gdzie bedzie etcd
w sekcji [kube-master] hosty typu master
w sekcji [kube-node] hosty typu worker

5. Deploy klastra :

   - przygotowanie ansible-playbook -i inventory/mycluster/inventory.ini kube-prep.yml
   - deploy klastra ansible-playbook -i inventory/mycluster/inventory.ini cluster.yml
6. weryfikacja deploya 
  - logujemy sie na mastera  ssh root@master.makieta.local
  - sprawdamy stan klastra   [root@master ~]# kubectl get nodes
			     NAME                   STATUS   ROLES    AGE     VERSION
			     master.makieta.local   Ready    master   10m     v1.18.3
                             node1.makieta.local    Ready    <none>   9m44s   v1.18.3
                             node2.makieta.local    Ready    <none>   9m44s   v1.18.3
         
                              
   
