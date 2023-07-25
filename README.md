# Développeur web Fullstack avec Spring et React

Ce cours, à travers le développement pas à pas d'une application d'emprunt de livre nommée "Sharebook", va vous permettre de mettre en pratique et comprendre à la fois la partie "backend", c'est à dire les traitements côté serveur, que "frontend", c'est à dire la partie visuelle de notre application.

Chaque session a un lien direct vers le code source concerné, afin de voir pas à pas si votre code est le bon. De plus, à la fin, on verra comment déployer notre application dans Azure! Bref la panel est large, et c'est le but de cette formation : vous faire appréhender toutes les briques essentielles du développement d'une Application Web moderne, avec les framework les plus utilisés en entreprise, SPRING et REACT.

La partie Backend, couvrira les aspects suivants :

- le framework SPRING CORE pour le fonctionnement de Spring;

- le framework SPRING MVC pour le développements de Web Services REST;

- le framework SPRING DATA pour interroger une base de données sans écrire de SQL (des notions de SQL sont les bienvenues cependant :)

- le framework SPRING SECURITY pour sécuriser notre application;



Mais ce cours couvre également la partie frontend, avec la mise en place d'une interface de type SPA (Single Page Application) moderne avec le framework React.

Ainsi on découvrira :

- la librairie React pour le développement frontend, avec notamment les principes fondamentaux de React que sont les props, le state, JSX, etc.

- les fondamentaux de Javascript à connaitre;

- Mettre en forme l'application avec Bootstrap

- L'appel au Backend et les APIs qu'on aura développées  ensemble, grâce à la librairie Axios et aux promesses Javascript;

- un aperçu de React-Bootstrap pour simplifier le coding de composants Bootstrap



Enfin pour le déploiement dans Azure, on étudiera :

- Azure App Services qui hébergera notre application Sharebook;

- Azure Vault pour stocker les informations secrètes;

- Github Actions pour déployer notre code;

- Un aperçu de la base de données Oracle dans le Cloud;

## Initialisation du backend avec Spring-Boot

<p align="center"><img src="img/archi.png" alt="Architecture" height="200px"/></p>

### Les fondamentaux de Maven

<p align="center"><img src="img/maven.png" alt="Maven" height="200px"/></p>

Maven s'occupe du cycle de vie de l'application, tout se passe par le fichier pom.xml.
Il s'occupe de toutes les dépendances, toutes les librairies tierces, comme Spring, ... 
Dans ce fichier on indique les paramètres pour la compilation qui va compiler, transcrire le code en exécutable.
Dans Maven tout est à base de pluggins.
S'occupe de la partie déploiement.

### Spring core

Se base sur un container Spring dans lequel on va déclarer des beans qui seront le coeur de notre application, par exemple, des services de DAO pour accèder à la base de données.
Structure de l'application qui contiendra le code métier.

<p align="center"><img src="img/SpringCore.png" alt="Spring" height="200px"/></p>

C'est beans Spring vont être initialisés au démarrage de l'application, c'est à travers un fichier XMl ou des annotations Java on a déclarer tous ses services ou beans que Spring va gérer.

On déclare avec @configuration une classe de configuration => rajouter au runtime du comportement dynamiquement. Déclarer un ou plusieurs beans, cela va dire à Spring crée moi un container avec ces beans là.
Une fois crée dans le code applicatif métier, on va pouvoir utiliser des instances directement de ces beans déclarés.
Dans config, on renvoie toujours une interface, et c'est l'implémentation qui sera retournée par Spring grâce à **@Autowired**.

### Spring Boot

<p align="center"><img src="img/SpringBoot.png" alt="SpringBoot" height="200px"/></p>

Permet d'initialiser l'application plus facilement grâce au principe: **convention over configuration**, il va faire des choix arbitraires sur l'application, et si l'on veut surcharger cela c'est possible. Il y a un mécanisme d'autoconfiguration en fonction des librairies que l'on utilise.

Fatjar quand on va builder l'application, on va embarquer un serveur directement, on aura juste à exécuter avec la commande Java, le jar qui contiendra le serveur d'application.
Tout le serveur sera préconfiguré.

Stareter grâce à Maven: avant déclaration de plein de dépendances, et maintenant utomatiquement Spring va importer tous les éléments.


### Initialisation du projet avec Spring Initializer

[Spring initializer](https://start.spring.io/)
Maven, Java, (snapshot= non finalisée)
Add dependencies: web pour appli web, permet de créer des controleurs REST
Generate => zip, ouvrir avec IDE.

pom.xml => fichier de maven qui va décrire l'ensemble de l'application.
On a u  parent Spring repose sur la notion de parent pour la gestion de dépendances Spring Boot, cette version de Spring Boot va déclarer nos dépendances.
On a juste à déclarer la brique web, la brique test et les dépendances sont automatiquement gérées.

src main java com udemy demo => première classe de démo: DemoApplication qui vient de lancer l'aplication.
méthode main, méthode centrale de Java.

@SpringBootApplication annotation SpringBoot => initialiser application , indique application SpringBoot.

resources => applications.properties: fichier de configuration de SpringBoot

### Compiler, tester, lancer l'application

On a initialisé notre projet avec Spring Initializer.

Commandes Maven:

mvn clean install => builder et installer le jar dans le repo local. Maven centralise tout ce que l'on a construit dans un repo local, qu'on va pouvoir aussi publier sur un repo distant.
cd target (jar)
java -jar demo -0.0.1-SNAPSHOT.jar (nom du jar) pour démarrer l'application, on va pouvoir lancer facilement des containers, dockers,...
mvn -test, se mettre dans demo

### Lancement via l'IDE

	package com.udemy.demo;

	import org.springframework.boot.SpringApplication;
	import org.springframework.boot.autoconfigure.SpringBootApplication;
	
	@SpringBootApplication
	public class DemoApplication {
	
		public static void main(String[] args) {
			SpringApplication.run(DemoApplication.class, args);
		}
	
	}
Point entrée application
[spring tools suit pour éclipse](https://spring.io/tools)

## Backend développement de l'API avec Sring MVC

<p align="center"><img src="img/Spring_MVC.png" alt="Spring MVC" height="200px"/></p>


### Présentation Spring MVC et RestFul


Développement avec Spring MVC.

User fait des requêtes au serveur backend via une API, point d'entrée avec tout les traitements métiers, on va développer des services REST qui suivent le paradigme REST.

API ensemble de service REST et constitue le point d'entrée de l' application.

RESTFUL:
**respecter au maximum le paradigme REST =  c'est utiliser les verbes HTTP, le protocole HTTP propose des verbes avec des rôles particuliers, code retour HTTP, formatJson, URL simple qui sera le point d'n=entrée, le endpoint du web service**
BookController ici.
On peut ajouter des paramètres dans l'URL = RequestParam. (clé=valeur)

### Les différents contrôleurs REST de l'application

Spring MVC va  nous fournir la notion de **contrôleur** = un élément qui va se trouver à la frontière entre le monde extérieur et la BDD, le système. Il faudra ajouter des composants, des beans: @RestController => décla d'un service REST automatiquement détecté et instancié par Spring.
Il suffit d'ajouter cette annotation en haut de la classe avec toutes nos méthode de contrôleur.

Les différents controlleurs REST:
- Book Controller, maj livre,
- User Controller, inscription,
- Borrow Controller, emprunt,
- JWT Controller, auth (plus contrôleur technique).

### Présentation du contrôleur book

Endpoint = URL qu'on va aller chercher pour ce service /books.
Pour tous ces services on connaît l'user connecté via son cookie JWT.
coquille: put/books

<p align="center"><img src="img/BookController.png" alt="Book Controller" height="200px"/></p>

### Développement du Book Controller

Création d'un package dans lequel on va stocker le book controller.
com.udemy.demo.book
class BookController @RestController permet de déclarer le bean spring, pour l'exposer au monde extérieur.
 
Création des endpoints pour afficher les livres disponbiles

get => @GetMapping(value="/books)
création class Book et class Category







