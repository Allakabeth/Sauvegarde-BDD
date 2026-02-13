# 🔄 GUIDE DE RESTAURATION - ACLEF Pédagogie

**Date du backup :** 2026-02-13 04:24:42
**Projet Supabase :** mkbchdhbgdynxwfhpxbw
**Statut :** PRODUCTION

---

## 📦 CONTENU DE CE BACKUP

- `database.sql` - **BASE COMPLÈTE** (structure + données du schéma public)
- `auth_users.sql` - Comptes utilisateurs et authentification
- `RESTORE.md` - Ce guide

---

## 🔄 RESTAURATION SIMPLE

### Commandes de restauration :

```bash
# 1. Définir l'URL de connexion
export DB_URL="postgresql://postgres.mkbchdhbgdynxwfhpxbw:[PASSWORD]@aws-1-eu-west-3.pooler.supabase.com:5432/postgres"

# 2. Restaurer la base complète (structure + données)
psql $DB_URL < database.sql

# 3. Restaurer les utilisateurs
psql $DB_URL < auth_users.sql
```

**C'est tout ! ✅**

---

## ⚠️ SCÉNARIOS D'UTILISATION

### Scénario A : Restauration sur le MÊME projet (Transparent)

**Quand utiliser :**
- Erreur humaine (DELETE accidentel)
- UPDATE destructeur
- Migration ratée
- Corruption de données
- Erreur de Claude Code

**Procédure :**
```bash
export DB_URL="postgresql://postgres.mkbchdhbgdynxwfhpxbw:aclefplanning2025@aws-1-eu-west-3.pooler.supabase.com:5432/postgres"
psql $DB_URL < database.sql
psql $DB_URL < auth_users.sql
```

**Résultat :**
- ✅ **Restauration transparente**
- ✅ Application continue de fonctionner
- ✅ Aucun changement d'URL nécessaire
- ⏱️ Temps : 10-15 minutes

---

### Scénario B : Restauration sur un NOUVEAU projet

**Quand utiliser :**
- Projet Supabase détruit
- Compte piraté
- Migration vers nouveau projet
- Catastrophe totale

**Procédure :**

1. **Créer nouveau projet Supabase**
   - Via https://supabase.com/dashboard
   - Notez le nouveau project-ref : `XXXXXXX`

2. **Récupérer la nouvelle URL**
   ```bash
   export NEW_DB_URL="postgresql://postgres.XXXXXXX:[PASSWORD]@aws-1-eu-west-3.pooler.supabase.com:5432/postgres"
   ```

3. **Restaurer les données**
   ```bash
   psql $NEW_DB_URL < database.sql
   psql $NEW_DB_URL < auth_users.sql
   ```

4. **Mettre à jour l'application**
   
   Fichier `.env.local` ou `.env.production` :
   ```env
   NEXT_PUBLIC_SUPABASE_URL=https://XXXXXXX.supabase.co
   NEXT_PUBLIC_SUPABASE_ANON_KEY=[nouvelle_anon_key]
   SUPABASE_SERVICE_ROLE_KEY=[nouvelle_service_role_key]
   ```

5. **Redéployer**
   ```bash
   vercel --prod  # ou votre méthode
   ```

**Résultat :**
- ✅ Toutes les données restaurées
- ✅ Tous les utilisateurs conservés
- ⚠️ Nouvelle URL (une seule fois)
- ⏱️ Temps total : 30-45 minutes

---

## 🔒 PROTECTION

### Ce backup protège contre :
- ✅ Erreurs humaines (DELETE, UPDATE, DROP)
- ✅ Migrations ratées
- ✅ Corruption de données
- ✅ Erreurs de Claude Code
- ✅ Destruction du projet Supabase
- ✅ Piratage / malveillance
- ✅ Catastrophes techniques

### Perte maximale de données :
- **24 heures** (backup quotidien à 2h00 UTC)

---

## 📊 INFORMATIONS TECHNIQUES

**Base de données :**
- 50+ tables dans le schéma public
- ~2000+ lignes de données
- RLS policies, functions, triggers inclus
- Extensions PostgreSQL incluses

**Fichiers :**
- `database.sql` : 500 Ko - 2 Mo (complet)
- `auth_users.sql` : 5-20 Ko (utilisateurs)

**Rétention :** 7 derniers backups conservés

**Versions :**
- PostgreSQL Server : 17.4
- pg_dump : 17.x (compatible)

---

## ✅ CHECKLIST POST-RESTAURATION

- [ ] Base de données restaurée sans erreur
- [ ] Tables présentes dans Supabase Dashboard
- [ ] Données visibles dans l'application
- [ ] Connexion utilisateur fonctionnelle
- [ ] RLS policies actives (vérifier dans Dashboard)
- [ ] Application accessible en production
- [ ] Tests de base réussis

---

## 🆘 EN CAS DE PROBLÈME

**Si la restauration échoue :**

1. Vérifiez que PostgreSQL est installé (`psql --version`)
2. Vérifiez l'URL de connexion (accessible via `psql $DB_URL`)
3. Vérifiez que les fichiers SQL ne sont pas corrompus (`wc -l *.sql`)
4. Essayez un backup plus ancien si nécessaire

**Si l'application ne fonctionne pas :**

1. Vérifiez les variables d'environnement
2. Vérifiez que les RLS policies sont actives
3. Consultez les logs Supabase (Dashboard > Logs)
4. Testez une requête simple depuis le Dashboard

---

**Votre production est protégée ! 🛡️**
