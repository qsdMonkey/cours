![GIT](image/angular.svg)
# Comprendre le cycle de vie d’un composant Angular ✨

Angular est un framework JavaScript puissant qui permet de créer des applications web dynamiques. Un des éléments centraux de ce framework est le **cycle de vie des composants**. Mais qu'est-ce que cela signifie exactement ? Et pourquoi est-ce important ? Pas de panique, on décortique tout cela ensemble !

---

## 🔹 Qu'est-ce que le cycle de vie d'un composant ?

Un composant Angular passe par plusieurs étapes depuis sa création jusqu'à sa destruction. Ces étapes constituent son **cycle de vie**. Angular propose des **hooks** (des méthodes spéciales) pour interagir à différents moments de ce cycle.

Ces hooks permettent de :

- Initialiser des données.
    
- Gérer les mises à jour.
    
- Nettoyer les ressources avant la destruction du composant.
    

Imaginez cela comme une pièce de théâtre : création du décor, entrée des acteurs, jeu scénique, puis baisser de rideau et nettoyage final.

---

## 🌱 Les grandes étapes du cycle de vie

Voici les étapes majeures du cycle de vie d’un composant Angular, accompagnées de leurs hooks associés :

### 1. **Création (Initialization)**

Le composant est créé et ses propriétés sont initialisées.

**Hooks utilisés :**

- `ngOnChanges(changes: SimpleChanges)` : Appelé à chaque fois qu’une propriété liée (à travers `@Input`) change.
    
- `ngOnInit()` : Idéal pour initialiser des données ou appeler des services. Ce hook est appelé une seule fois juste après la création du composant.
    

> **Astuce :** Utilisez `ngOnInit` pour placer vos appels d'API ou initialiser des états complexes.

---

### 2. **Mise à jour (Change Detection)**

Le composant réagit aux modifications des données ou des propriétés.

**Hooks utilisés :**

- `ngDoCheck()` : Appelé à chaque détection de changement. Ce hook est plus puissant que `ngOnChanges` mais peut être coûteux en performances.
    
- `ngAfterContentInit()` : Appelé une fois après l’insertion du contenu projeté (via `ng-content`).
    
- `ngAfterContentChecked()` : Appelé à chaque fois que le contenu projeté est vérifié.
    
- `ngAfterViewInit()` : Appelé une fois après l’initialisation de la vue du composant.
    
- `ngAfterViewChecked()` : Appelé à chaque fois que la vue est vérifiée.
    

> **⚠ Attention :** Abuser de `ngDoCheck` peut ralentir votre application. Utilisez-le avec parcimonie.

---

### 3. **Destruction (Destruction)**

Quand un composant est retiré de la vue, Angular nettoie les ressources associées.

**Hook utilisé :**

- `ngOnDestroy()` : Idéal pour désouscrire d’un Observable ou supprimer des écouteurs d’événements.
    

> **Astuce :** Toujours nettoyer vos Observables avec `ngOnDestroy` pour éviter les fuites mémoires ! 🕵️‍♂️

---
![cycle](angular-component-lifecycle.png)

## 🔧 Exercice pratique : Le composant vigilant

**Objectif :** Créez un composant Angular qui suit ces étapes de cycle de vie et affiche des logs dans la console à chaque étape.

1. Créez un nouveau composant :
    
    ```bash
    ng generate component VigilantComponent
    ```
    
2. Modifiez le fichier TypeScript pour implémenter tous les hooks :
    
    ```typescript
    import { Component, OnInit, OnChanges, DoCheck, AfterContentInit, AfterContentChecked, AfterViewInit, AfterViewChecked, OnDestroy, Input } from '@angular/core';
    
    @Component({
      selector: 'app-vigilant',
      template: `<p>Vigilant works! <button (click)="triggerChange()">Changer les données</button></p>`
    })
    export class VigilantComponent implements OnInit, OnChanges, DoCheck, AfterContentInit, AfterContentChecked, AfterViewInit, AfterViewChecked, OnDestroy {
      @Input() data: string;
    
      constructor() { console.log('Constructor: Le composant est instancié.'); }
    
      ngOnChanges() { console.log('ngOnChanges: Les entrées ont changé.'); }
      ngOnInit() { console.log('ngOnInit: Initialisation du composant.'); }
      ngDoCheck() { console.log('ngDoCheck: Détection de changement.'); }
      ngAfterContentInit() { console.log('ngAfterContentInit: Contenu projeté initialisé.'); }
      ngAfterContentChecked() { console.log('ngAfterContentChecked: Contenu projeté vérifié.'); }
      ngAfterViewInit() { console.log('ngAfterViewInit: Vue initialisée.'); }
      ngAfterViewChecked() { console.log('ngAfterViewChecked: Vue vérifiée.'); }
      ngOnDestroy() { console.log('ngOnDestroy: Le composant est détruit.'); }
    
      triggerChange() {
        this.data = 'Nouvelle valeur';
        console.log('Données changées via le bouton.');
      }
    }
    ```
    
3. Modifiez le fichier HTML du parent pour inclure le composant :
    
    ```html
    <app-vigilant [data]="message"></app-vigilant>
    <button (click)="message = 'Message mis à jour!'">Mettre à jour le message</button>
    ```
    
    Dans le fichier TypeScript parent :
    
    ```typescript
    export class ParentComponent {
      message = 'Message initial';
    }
    ```
    

---

## 📊 Récapitulatif

|Étape|Hook|Description|
|---|---|---|
|**Création**|`ngOnChanges`, `ngOnInit`|Initialisation du composant|
|**Mise à jour**|`ngDoCheck`, `ngAfterContentInit`, `ngAfterContentChecked`, `ngAfterViewInit`, `ngAfterViewChecked`|Gestion des modifications|
|**Destruction**|`ngOnDestroy`|Nettoyage des ressources|