# 👥 Rôles et Permissions - Site Caméléon V2

## Hiérarchie Complète des Rôles

### 1. **Founder** (Créateur du site)
- ✅ Accès absolu à tout
- ✅ Créer/modifier/supprimer des rôles
- ✅ Changer de mode du site
- ✅ Gérer tous les membres du staff
- ✅ Visualiser tous les logs
- ✅ Budgets illimités

### 2. **Co-Owner** (Copropriétaire)
- ✅ Presque tous les droits du Founder
- ✅ Changer de mode du site
- ✅ Gérer le staff (sauf Founder)
- ✅ Visualiser tous les logs
- ❌ Créer des rôles
- ❌ Supprimer le Founder

### 3. **Admin** (Administrateur)
- ✅ Changer de mode du site
- ✅ Ajouter/retirer des utilisateurs
- ✅ Gérer les applications
- ✅ Ajouter les modérateurs
- ✅ Voir les logs
- ❌ Modifier les rôles existants
- ❌ Gérer les Founders/Co-Owners

### 4. **Manager Staff** (Gestionnaire d'équipe)
- ✅ Ajouter/retirer du staff (tous rôles sauf Admin)
- ✅ Voir les logs du staff
- ✅ Modifier les permissions du staff
- ✅ Créer des sous-équipes
- ❌ Changer de mode
- ❌ Modifier les applications

### 5. **Manager Tout** (Omnigérant)
- ✅ Tout faire sauf changer le mode du site
- ✅ Gérer les applications
- ✅ Gérer le contenu
- ✅ Voir les logs
- ✅ Ajouter du staff
- ❌ Changer le mode
- ❌ Modifier les rôles globaux

### 6. **Manager Designer** (Responsable design)
- ✅ Superviser les designers
- ✅ Approuver les changements visuels
- ✅ Modifier les thèmes
- ✅ Ajouter des designers
- ✅ Voir le portfolio des designs
- ❌ Changer le mode
- ❌ Accéder aux données utilisateur

### 7. **Designer** (Graphiste)
- ✅ Modifier les thèmes visuels
- ✅ Éditer les couleurs/polices
- ✅ Créer des variantes de thèmes
- ✅ Voir l'aperçu en direct
- ✅ Soumettre des designs pour approbation
- ❌ Activer un design (besoin Manager Designer)
- ❌ Voir les données utilisateur

### 8. **Coder** (Développeur)
- ✅ Accès au code source
- ✅ Créer des branches de développement
- ✅ Soumettre des PR
- ✅ Voir les logs techniques
- ✅ Accéder à la documentation API
- ❌ Merger en production (besoin Co-Owner)
- ❌ Accéder à la base de données directe

### 9. **Tester** (QA)
- ✅ Accès aux environnements de test
- ✅ Reporter des bugs
- ✅ Voir les logs des tests
- ✅ Tester toutes les fonctionnalités
- ✅ Créer des rapports de test
- ❌ Modifier le code
- ❌ Déployer

### 10. **Modérateur** (Basique)
- ✅ Accepter/refuser les applications
- ✅ Voir les candidatures en attente
- ✅ Ajouter des notes aux candidatures
- ✅ Bannir/débannir des utilisateurs
- ✅ Voir ses actions en log
- ❌ Voir les données privées
- ❌ Modifier le contenu du site

## Matrice des Permissions

| Permission | Founder | Co-Owner | Admin | Mgr Staff | Mgr Tout | Mgr Design | Designer | Coder | Tester | Mod |
|---|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| Changer Mode | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ |
| Gérer Staff | ✅ | ✅ | ✅ | ✅ | ✅ | ⚠️ | ❌ | ❌ | ❌ | ❌ |
| Voir Logs | ✅ | ✅ | ✅ | ✅ | ✅ | ❌ | ❌ | ⚠️ | ⚠️ | ⚠️ |
| Modifier Thème | ✅ | ✅ | ⚠️ | ❌ | ⚠️ | ✅ | ✅ | ❌ | ❌ | ❌ |
| Gérer Applications | ✅ | ✅ | ✅ | ❌ | ✅ | ❌ | ❌ | ❌ | ❌ | ✅ |
| Accès Code | ✅ | ✅ | ⚠️ | ❌ | ❌ | ❌ | ❌ | ✅ | ✅ | ❌ |
| Déployer | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ |

*Legend: ✅ = Full access, ⚠️ = Limited access, ❌ = No access*

## Flux d'Autorisation

```typescript
// Exemple: Changer le mode du site

if (user.hasPermission('MODE_SWITCH')) {
  // Seul Founder, Co-Owner, Admin peuvent
  switchMode(newMode);
} else {
  throw new UnauthorizedError();
}

// Exemple: Accepter une candidature

if (user.hasPermission('REVIEW_APPLICATIONS')) {
  // Founder, Co-Owner, Admin, Manager Tout, Modérateur
  approveApplication(appId);
} else {
  throw new UnauthorizedError();
}
```
