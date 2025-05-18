<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Déployer OpenStack All-in-One avec Kolla-Ansible</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 20px;
            line-height: 1.6;
            background-color: #f9f9f9;
            color: #333;
        }
        h1, h2, h3 {
            color: #2c3e50;
        }
        h1 {
            font-size: 2.5em;
            margin-bottom: 10px;
        }
        h2 {
            font-size: 2em;
            margin-top: 20px;
            margin-bottom: 10px;
        }
        h3 {
            font-size: 1.5em;
            margin-top: 15px;
            margin-bottom: 5px;
        }
        p {
            margin: 10px 0;
        }
        ul {
            margin: 10px 0;
            padding-left: 20px;
        }
        code {
            background-color: #eaeaea;
            padding: 2px 4px;
            border-radius: 4px;
        }
        pre {
            background-color: #eaeaea;
            padding: 10px;
            border-radius: 4px;
            overflow-x: auto;
        }
        .footer {
            margin-top: 40px;
            font-size: 0.9em;
            color: #7f8c8d;
        }
    </style>
</head>
<body>

<h1>Déployer OpenStack All-in-One avec Kolla-Ansible</h1>

<h2>Introduction</h2>
<p>Dans cet article, nous allons explorer comment déployer une instance d'OpenStack All-in-One en utilisant Kolla-Ansible. Ce guide est destiné aux développeurs et aux administrateurs système qui souhaitent expérimenter avec OpenStack dans un environnement local.</p>

<h2>Prérequis</h2>
<p>Avant de commencer, assurez-vous d'avoir :</p>
<ul>
    <li><strong>UBUNTU 24.04 Server LTS </strong> installé sur votre machine.</li>
    <li>Minimum system requirements for running OpenStack</li>
    <li>-- CPU : 2 (Recommended 4)</li>
    <li>-- RAM : 10GB (Recommended 16 GB)</li>
    <li>-- Disk space : 60 GB (Recommended 100 GB)</li>
    <li><strong>Client SSH</strong> pour faciliter la gestion de la machine virtuelle.</li>
    <li><strong>Une connexion Internet </strong> pour télécharger les images et les dépendances nécessaires.</li>
</ul>

<h2>Étape 1 : Configurer l'environnement</h2>

<h3>1.1 Installer Ubuntu et mettre à jour le systeme</h3>
<pre><code>sudo su
 apt update && apt -y upgrade
 apt autoremove</code></pre>

<h2>Étape 2 : Install Dependencies on the VM</h2>
<h3>2.1 Configure static IP in netplan configuration file located at /etc/netplan/ and deploy the new configuration:</h3>
<li>Use nano for editing 50-cloud-init.yaml file.</li>

<pre><code>nano /etc/netplan/50-cloud-init.yaml</code></pre>

<li>Modify the file like below. Replace the IP with your actual IP.</li>

<li>enp0s3 : Internal Netword</li>
<li>enp0s8 : External Network</li>

<pre><code>network:
    version: 2
    renderer: networkd
    ethernets:
        enp0s3:
            addresses:
              - 10.30.48.10/22
            nameservers:
              addresses: [8.8.8.8]
            routes:
              - to: default
                via:  10.30.51.254
        enp0s8:
            dhcp4: true</code></pre>

<li>Apply new configuration and check IP address.</li>

<pre><code>netplan apply
ip addr</code></pre>


<h2>Étape 2 : Déployer Kolla-Ansible</h2>

<h3>2.1 Cloner le dépôt Kolla-Ansible</h3>
<p>Clonez le dépôt Kolla-Ansible depuis GitHub :</p>
<pre><code>git clone https://github.com/openstack/kolla-ansible.git
cd kolla-ansible</code></pre>

<h3>2.2 Installer les dépendances</h3>
<p>Installez les dépendances nécessaires avec pip :</p>
<pre><code>pip install -r requirements.txt</code></pre>

<h3>2.3 Configurer Kolla</h3>
<p>Copiez le fichier de configuration par défaut :</p>
<pre><code>cp -r etc/kolla /etc/kolla</code></pre>
<p>Modifiez le fichier de configuration <code>globals.yml</code> pour adapter les paramètres selon vos besoins.</p>

<h2>Étape 3 : Lancer OpenStack</h2>

<h3>3.1 Initialiser le déploiement</h3>
<p>Initialisez votre déploiement avec Kolla-Ansible :</p>
<pre><code>kolla-ansible bootstrap-servers</code></pre>

<h3>3.2 Déployer OpenStack</h3>
<p>Lancez le déploiement avec la commande suivante :</p>
<pre><code>kolla-ansible deploy</code></pre>

<h2>Étape 4 : Accéder à OpenStack</h2>

<h3>Accéder à l'interface Horizon</h3>
<p>Après le déploiement, vous pouvez accéder à l'interface Horizon d'OpenStack via votre navigateur à l'adresse suivante :</p>
<pre><code>http://&lt;adresse-ip-de-votre-machine-virtuelle&gt;/horizon</code></pre>

<h2>Conclusion</h2>
<p>Vous avez maintenant un environnement OpenStack All-in-One fonctionnel sur VirtualBox grâce à Kolla-Ansible. Vous pouvez commencer à explorer les fonctionnalités d'OpenStack et à développer des applications cloud.</p>

<h2>Ressources supplémentaires</h2>
<ul>
    <li><a href="https://docs.openstack.org/">Documentation officielle d'OpenStack</a></li>
    <li><a href="https://github.com/openstack/kolla-ansible">Kolla-Ansible GitHub</a></li>
</ul>

<div class="footer">
    <p>Publié par [Votre Nom]</p>
    <p>Date : [Date de publication]</p>
</div>

</body>
</html>
