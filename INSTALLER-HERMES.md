# Feuille de guidance — Installer « Hermès » (le super pouvoir) sur un nouveau PC

> **À quoi sert ce document ?**
> Reproduire, sur le PC d'une autre personne, l'atelier **Hermès** :
> 1. **Claude Code** = l'IA agentique puissante (accès aux fichiers, code, exécution) — *le « super pouvoir »* ;
> 2. un **chat local Ollama** = une IA d'appoint **gratuite et hors-ligne** (le repli) ;
> 3. une **voie cloud dormante** (clé API) déjà câblée, avec garde-fou facture.
>
> Le document se lit en **deux temps** :
> - **PARTIE A — pour l'humain** : ce qu'il faut installer *avant* (compte, logiciels). À faire à la main.
> - **PARTIE B — pour Claude** : on lance Claude Code, et on lui dit *« lis ce fichier et exécute la PARTIE B »*. Claude crée le dossier, les fichiers et les raccourcis tout seul.

---

## ⚠️ Lis ceci d'abord (résumé honnête)

- **Claude Code n'est pas gratuit.** Il faut un **compte Claude payant** (la formule **Max** est recommandée pour Claude Code ; au minimum un abonnement qui ouvre Claude Code). Sans abonnement, le « super pouvoir » ne fonctionne pas — il reste seulement le chat Ollama local (gratuit).
- **Le chat Ollama est gratuit mais gourmand.** Les modèles pèsent plusieurs Go (le modèle généraliste `mistral-nemo:12b` ≈ 7 Go) et demandent une machine correcte (idéalement **16 Go de RAM**, et de préférence une carte graphique). Sur un petit PC, on prendra des modèles légers (`gemma3:4b`, `qwen2.5:3b`).
- **Claude Code = cloud.** Il a besoin d'**internet** et envoie ce sur quoi il travaille vers Anthropic. À l'inverse, le chat Ollama est **100 % local** (rien ne sort du PC).

---

# PARTIE A — Pour l'humain (à faire AVANT de lancer Claude)

### 1) Créer un compte Claude + prendre un abonnement
1. Aller sur **https://claude.ai** → **Sign up**, créer le compte (e-mail + mot de passe).
2. Souscrire une formule payante (**Max** conseillée pour Claude Code) depuis **Settings → Billing**.

### 2) Installer Claude Code (l'appli de bureau)
1. Télécharger l'appli de bureau **Claude** pour Windows : **https://claude.ai/download**.
2. L'installer, **se connecter** avec le compte créé à l'étape 1.
3. Lancer **Claude Code** au moins une fois (depuis l'appli) et accepter la connexion.
   *Claude Code s'installe avec l'appli, en général sous `C:\Users\<TONNOM>\AppData\Roaming\Claude\claude-code\<version>\claude.exe`. Claude saura retrouver ce chemin tout seul en PARTIE B.*

### 3) Installer Node.js (pour le chat local)
1. Télécharger la version **LTS** sur **https://nodejs.org** → installer (tout par défaut).
2. Vérification (facultatif) : ouvrir **PowerShell** et taper `node --version` → un numéro doit s'afficher.

### 4) Installer Ollama + télécharger les modèles (pour le chat local gratuit)
1. Télécharger Ollama sur **https://ollama.com/download** → installer.
2. Ouvrir **PowerShell** et télécharger au moins un modèle (commence par un léger si le PC est modeste) :
   ```powershell
   ollama pull gemma3:4b
   ollama pull qwen2.5:3b
   ```
   Si la machine est puissante (16 Go+ RAM), ajouter les deux meilleurs :
   ```powershell
   ollama pull mistral-nemo:12b
   ollama pull qwen2.5-coder:7b
   ```
   *(Sans aucun modèle, le chat local s'ouvrira mais n'aura rien à proposer. Claude Code, lui, fonctionne sans Ollama.)*

### 5) Lancer Claude Code, puis lui donner ce fichier
- Ouvrir Claude Code, et lui écrire :
  > **« Lis le fichier `GUIDANCE-HERMES-beau-frere.md` et exécute la PARTIE B. Mon prénom est ____ . »**

C'est tout pour l'humain. La suite, c'est Claude qui la fait.

---

# PARTIE B — Pour Claude (instructions à exécuter)

**Claude, tu vas installer l'atelier « Hermès » sur ce PC.** Suis ces étapes dans l'ordre. Si tu n'as pas le prénom de l'utilisateur, **demande-le d'abord** : tu en as besoin pour personnaliser l'identité (remplace `[PRÉNOM]` partout ci-dessous).

### Étape 1 — Créer l'arborescence
Crée, **hors OneDrive** (pour éviter la corruption de synchro), le dossier `C:\Hermes` avec :
```
C:\Hermes\
  chat\              (le serveur de chat local + la page web)
  conversations\     (sauvegardes de conversations en .md, créé tout seul)
  travaux\           (les productions, un sous-dossier par projet)
```

### Étape 2 — Écrire `C:\Hermes\CLAUDE.md` (l'identité de l'IA agentique)
Crée ce fichier **en remplaçant `[PRÉNOM]`** par le prénom de l'utilisateur :

````markdown
# Hermès — assistant généraliste libre

Tu es **Hermès**, l'assistant agentique **généraliste et libre** de [PRÉNOM], sur son PC.
C'est son espace **personnel et débridé** où il vient **produire, créer, coder, réfléchir**
librement.

## Ta posture
- **Généraliste et proactif.** Code, rédaction, automatisation, idées, analyse — tout est permis.
- Tu **agis** : tu proposes, tu construis, tu vas au bout. Pas besoin de demander la permission
  pour chaque micro-étape (pour les actions sensibles ou irréversibles, oui, on confirme).
- **Honnête et direct.** Si une idée est bancale ou irréaliste, tu le dis franchement.

## Prudence
- Pour les actions difficilement réversibles ou tournées vers l'extérieur (supprimer, publier,
  envoyer), tu confirmes d'abord.
- Aucune donnée personnelle sensible ne part vers un service externe sans accord explicite.
  En cas de doute : tu demandes.

## L'utilisateur
[PRÉNOM] — usage **généraliste et personnel** de cet espace.

## Langue
**Français**, orthographe et accents soignés. Les termes techniques et les identifiants de code
restent dans leur forme d'origine.

## Tes travaux
Range les productions dans `C:\Hermes\travaux\` (un sous-dossier par projet). Hermès vit hors
OneDrive (pas de synchro à risque) — un dépôt git par projet si besoin.
````

### Étape 3 — Écrire `C:\Hermes\chat\serveur.js`
Petit serveur Node **sans aucune dépendance** : il sert la page de chat, parle à Ollama (local),
et gère la voie cloud dormante avec garde-fou facture. Écris-le **tel quel** :

````javascript
// Hermès local — petit serveur de chat branché sur Ollama (100 % local, gratuit).
// Aucune dépendance : modules natifs de Node seulement. Le navigateur parle à CE
// serveur (même origine → pas de souci CORS) ; le serveur parle à Ollama et
// enregistre les conversations sur le disque (C:\Hermes\conversations).
const http = require('http');
const https = require('https');
const fs = require('fs');
const path = require('path');
const { exec } = require('child_process');

const PORT = 4545;
const OLLAMA = { host: '127.0.0.1', port: 11434 };
const DIR = __dirname;                                   // C:\Hermes\chat
const CONV_DIR = path.join(path.dirname(DIR), 'conversations'); // C:\Hermes\conversations
try { fs.mkdirSync(CONV_DIR, { recursive: true }); } catch (e) {}

// ─── Voie CLOUD (clés API) — CÂBLÉE MAIS DORMANTE ───────────────────────────
// Aujourd'hui : 100 % local (Ollama). Pour activer une IA cloud plus tard, poser
// une clé dans C:\Hermes\config.json → { "cloud": { "anthropic": { "cle": "sk-..." } } }.
// GARDE-FOU FACTURE : aucun appel cloud ne part sans confirmation explicite (confirmePayant).
const CONFIG_PATH = path.join(path.dirname(DIR), 'config.json'); // C:\Hermes\config.json
function chargerConfig() { try { return JSON.parse(fs.readFileSync(CONFIG_PATH, 'utf8')); } catch (e) { return {}; } }
// Tarifs indicatifs (USD / million de tokens) → estimation du coût de CHAQUE appel.
const TARIFS = {
  'claude-opus-4-8':   { in: 5, out: 25 },
  'claude-sonnet-4-6': { in: 3, out: 15 },
  'claude-haiku-4-5':  { in: 1, out: 5 },
};
function anthropicConf() {
  var a = (chargerConfig().cloud || {}).anthropic || {};
  return { cle: a.cle || '', modeles: a.modeles || Object.keys(TARIFS), max_tokens: a.max_tokens || 4096 };
}
function appelAnthropic(cle, payload) {
  return new Promise((ok, ko) => {
    const data = JSON.stringify(payload);
    const r = https.request({
      hostname: 'api.anthropic.com', path: '/v1/messages', method: 'POST',
      headers: { 'content-type': 'application/json', 'x-api-key': cle, 'anthropic-version': '2023-06-01', 'content-length': Buffer.byteLength(data) },
    }, (rp) => { let b = ''; rp.on('data', c => b += c); rp.on('end', () => ok({ status: rp.statusCode, body: b })); });
    r.on('error', ko); r.write(data); r.end();
  });
}

function send(res, code, type, body) { res.writeHead(code, { 'Content-Type': type }); res.end(body); }
function lireCorps(req) { return new Promise((ok) => { let d = ''; req.on('data', c => d += c); req.on('end', () => ok(d)); }); }

// Appel à Ollama (local).
function ollama(chemin, methode, charge) {
  return new Promise((ok, ko) => {
    const data = charge ? JSON.stringify(charge) : null;
    const opt = { host: OLLAMA.host, port: OLLAMA.port, path: chemin, method: methode, headers: {} };
    if (data) { opt.headers['Content-Type'] = 'application/json'; opt.headers['Content-Length'] = Buffer.byteLength(data); }
    const r = http.request(opt, (rp) => { let b = ''; rp.on('data', c => b += c); rp.on('end', () => ok({ status: rp.statusCode, body: b })); });
    r.on('error', ko);
    if (data) r.write(data);
    r.end();
  });
}

const serveur = http.createServer(async (req, res) => {
  try {
    if (req.method === 'GET' && (req.url === '/' || req.url === '/index.html')) {
      return send(res, 200, 'text/html; charset=utf-8', fs.readFileSync(path.join(DIR, 'index.html')));
    }
    if (req.method === 'GET' && req.url === '/api/modeles') {
      var liste = [];
      try {
        const o = await ollama('/api/tags', 'GET', null);
        (JSON.parse(o.body).models || []).map(m => m.name).filter(n => !/embed/i.test(n))
          .forEach(n => liste.push({ nom: n, type: 'local', dispo: true }));
      } catch (e) { /* Ollama éteint : aucun modèle local */ }
      var a = anthropicConf();
      a.modeles.forEach(m => liste.push({ nom: m, type: 'cloud', dispo: !!a.cle }));
      return send(res, 200, 'application/json', JSON.stringify({ modeles: liste }));
    }
    if (req.method === 'POST' && req.url === '/api/chat') {
      const body = JSON.parse(await lireCorps(req));
      const a = anthropicConf();
      const estCloud = a.modeles.indexOf(body.model) !== -1;
      if (estCloud) {
        // GARDE-FOU FACTURE : pas de clé → désactivé ; pas de confirmation → on refuse l'appel.
        if (!a.cle) return send(res, 200, 'application/json', JSON.stringify({ erreur: 'Modèle cloud non activé : aucune clé API. Ajoute ta clé dans C:\\Hermes\\config.json (cloud.anthropic.cle).' }));
        if (!body.confirmePayant) return send(res, 200, 'application/json', JSON.stringify({ besoinConfirmation: true, message: 'Cet appel utilise ' + body.model + ' (cloud, FACTURÉ sur ta clé API). Confirmer ?' }));
        const payload = {
          model: body.model, max_tokens: a.max_tokens,
          system: 'Tu es Hermès, l\'assistant généraliste de l\'utilisateur. Réponds en français, de façon claire et directe.',
          messages: (body.messages || []).filter(m => m.role === 'user' || m.role === 'assistant'),
        };
        const r = await appelAnthropic(a.cle, payload);
        let data = {}; try { data = JSON.parse(r.body); } catch (e) {}
        if (r.status !== 200) return send(res, 200, 'application/json', JSON.stringify({ erreur: 'Erreur API (' + r.status + ') : ' + ((data.error && data.error.message) || String(r.body).slice(0, 200)) }));
        const texte = (data.content || []).filter(b => b.type === 'text').map(b => b.text).join('');
        const t = TARIFS[body.model], u = data.usage;
        const coutUsd = (t && u) ? (u.input_tokens / 1e6 * t.in + u.output_tokens / 1e6 * t.out) : null;
        return send(res, 200, 'application/json', JSON.stringify({ content: texte, usage: u || null, coutUsd: coutUsd }));
      }
      // Modèle LOCAL (Ollama) — gratuit, aucune confirmation.
      const o = await ollama('/api/chat', 'POST', { model: body.model, messages: body.messages, stream: false });
      const data = JSON.parse(o.body);
      return send(res, 200, 'application/json', JSON.stringify({ content: (data.message && data.message.content) || '', erreur: data.error || null }));
    }
    if (req.method === 'POST' && req.url === '/api/save') {
      const body = JSON.parse(await lireCorps(req));
      let titre = String(body.titre || 'conversation').replace(/[^\p{L}\p{N} _-]/gu, '').trim().slice(0, 60) || 'conversation';
      const d = new Date();
      const pad = n => String(n).padStart(2, '0');
      const stamp = `${d.getFullYear()}-${pad(d.getMonth() + 1)}-${pad(d.getDate())}_${pad(d.getHours())}h${pad(d.getMinutes())}`;
      const fichier = path.join(CONV_DIR, `${stamp}-${titre}.md`);
      fs.writeFileSync(fichier, String(body.contenu || ''), 'utf8');
      return send(res, 200, 'application/json', JSON.stringify({ ok: true, fichier }));
    }
    send(res, 404, 'text/plain; charset=utf-8', 'introuvable');
  } catch (e) {
    send(res, 500, 'application/json', JSON.stringify({ erreur: String((e && e.message) || e) }));
  }
});

serveur.listen(PORT, '127.0.0.1', () => {
  console.log('Hermes (local) pret sur http://127.0.0.1:' + PORT + '  (Ctrl+C ou fermer la fenetre pour arreter)');
  if (!process.env.HERMES_NOBROWSER) exec('start "" http://127.0.0.1:' + PORT);
});
````

### Étape 4 — Écrire `C:\Hermes\chat\index.html` (la page de chat)
Écris-la **telle quelle** :

````html
<!doctype html>
<html lang="fr">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Hermès — assistant local</title>
<style>
  :root{ --bleu:#1b3a63; --bleu2:#12263f; --orange:#ff6b35; --bg:#0f1d33; --bulle:#16294aff; }
  *{ box-sizing:border-box; }
  body{ margin:0; font-family:'Segoe UI',system-ui,sans-serif; background:var(--bg); color:#e8eef7; height:100vh; display:flex; flex-direction:column; }
  header{ background:linear-gradient(135deg,var(--bleu),#2c5f9e); padding:10px 16px; display:flex; align-items:center; gap:12px; box-shadow:0 2px 10px rgba(0,0,0,.3); }
  header .titre{ font-weight:800; font-size:1.05rem; flex:1; }
  header .titre small{ font-weight:400; opacity:.8; font-size:.72rem; display:block; }
  select,button{ font:inherit; }
  select{ background:#0f1d33; color:#e8eef7; border:1px solid #3a5680; border-radius:8px; padding:6px 10px; }
  #conv{ flex:1; overflow-y:auto; padding:18px; display:flex; flex-direction:column; gap:12px; }
  .msg{ max-width:80%; padding:10px 14px; border-radius:12px; line-height:1.5; white-space:pre-wrap; word-wrap:break-word; }
  .moi{ align-self:flex-end; background:var(--orange); color:#fff; border-bottom-right-radius:3px; }
  .hal{ align-self:flex-start; background:#1c335a; border:1px solid #2c4a72; border-bottom-left-radius:3px; }
  .meta{ font-size:.7rem; opacity:.65; margin-bottom:2px; }
  .vide{ margin:auto; text-align:center; opacity:.6; max-width:420px; }
  footer{ padding:12px 16px; border-top:1px solid #23344f; display:flex; gap:10px; align-items:flex-end; background:#0d1a2e; }
  textarea#saisie{ flex:1; resize:none; min-height:46px; max-height:160px; background:#0f1d33; color:#e8eef7; border:1px solid #3a5680; border-radius:10px; padding:11px 13px; font:inherit; }
  button.principal{ background:var(--orange); color:#fff; border:none; border-radius:10px; padding:12px 18px; font-weight:700; cursor:pointer; }
  button.principal:disabled{ opacity:.5; cursor:default; }
  button.fant{ background:transparent; color:#cdd9ec; border:1px solid #3a5680; border-radius:8px; padding:6px 12px; cursor:pointer; }
  .etat{ font-size:.78rem; opacity:.7; padding:0 16px 8px; }
</style>
</head>
<body>
  <header>
    <span style="font-size:1.4rem">⚜️</span>
    <div class="titre">Hermès <small>assistant local — Ollama (hors ligne)</small></div>
    <select id="modele" title="Modèle local"></select>
    <button class="fant" id="effacer" title="Nouvelle conversation">Nouveau</button>
    <button class="fant" id="enregistrer" title="Enregistrer sur le disque">💾 Enregistrer</button>
  </header>
  <div id="conv">
    <div class="vide" id="accueil">
      <p style="font-size:2rem;margin:0">⚜️</p>
      <p><strong>Hermès</strong> — ton IA locale, libre et gratuite.<br>Rien ne sort de ton PC. Pose ta question, demande du code, de la rédaction…</p>
    </div>
  </div>
  <div class="etat" id="etat"></div>
  <footer>
    <textarea id="saisie" placeholder="Écris ici… (Entrée pour envoyer, Maj+Entrée pour aller à la ligne)"></textarea>
    <button class="principal" id="envoyer">Envoyer</button>
  </footer>

<script>
  var historique = [];
  var conv = document.getElementById('conv');
  var saisie = document.getElementById('saisie');
  var btnEnvoyer = document.getElementById('envoyer');
  var selModele = document.getElementById('modele');
  var etat = document.getElementById('etat');

  function esc(s){ return String(s).replace(/[&<>]/g, c=>({'&':'&amp;','<':'&lt;','>':'&gt;'}[c])); }

  function bulle(role, texte){
    var ac = document.getElementById('accueil'); if (ac) ac.remove();
    var d = document.createElement('div');
    d.className = 'msg ' + (role==='user'?'moi':'hal');
    d.innerHTML = '<div class="meta">' + (role==='user'?'Moi':'Hermès') + '</div>' + esc(texte);
    conv.appendChild(d); conv.scrollTop = conv.scrollHeight;
    return d;
  }

  var infosModeles = {};
  async function chargerModeles(){
    try{
      var r = await fetch('/api/modeles'); var j = await r.json();
      var mods = j.modeles || [];
      selModele.innerHTML = ''; infosModeles = {};
      mods.forEach(function(m){
        infosModeles[m.nom] = m;
        var o = document.createElement('option');
        o.value = m.nom;
        o.textContent = m.nom + (m.type==='cloud' ? (m.dispo ? '  ☁ payant' : '  🔒 clé requise') : '');
        if (m.type==='cloud' && !m.dispo) o.disabled = true;
        selModele.appendChild(o);
      });
      var locaux = mods.filter(function(m){ return m.type==='local'; });
      var prefere = locaux.find(function(m){ return /mistral-nemo/.test(m.nom); }) || locaux[0];
      if (prefere) selModele.value = prefere.nom;
      if (!locaux.length) etat.textContent = 'Aucun modèle local. Vérifie qu\'Ollama est lancé (ex. : ollama pull mistral-nemo).';
    }catch(e){ etat.textContent = 'Serveur/Ollama injoignable.'; }
  }

  async function envoyer(){
    var texte = saisie.value.trim(); if (!texte) return;
    var modele = selModele.value;
    var info = infosModeles[modele] || { type:'local' };
    var confirmePayant = false;
    if (info.type === 'cloud') {
      if (!info.dispo) { etat.textContent = 'Ce modèle cloud n\'est pas activé (clé API manquante dans config.json).'; return; }
      if (!confirm('⚠ ' + modele + ' est une IA CLOUD, FACTURÉE sur ta clé API.\n\nEnvoyer cet appel payant ?')) { etat.textContent = 'Appel cloud annulé.'; return; }
      confirmePayant = true;
    }
    saisie.value=''; saisie.style.height='auto';
    bulle('user', texte);
    historique.push({role:'user', content:texte});
    btnEnvoyer.disabled = true;
    etat.textContent = (info.type==='cloud') ? ('Appel à ' + modele + ' (cloud)…') : 'Hermès réfléchit… (modèle local, quelques secondes)';
    try{
      var r = await fetch('/api/chat', { method:'POST', headers:{'Content-Type':'application/json'}, body: JSON.stringify({ model: modele, messages: historique, confirmePayant: confirmePayant }) });
      var j = await r.json();
      if (j.besoinConfirmation){ etat.textContent = j.message || 'Confirmation requise.'; historique.pop(); }
      else if (j.erreur){ etat.textContent = 'Erreur : ' + j.erreur; historique.pop(); }
      else {
        bulle('assistant', j.content); historique.push({role:'assistant', content:j.content});
        if (j.coutUsd != null) etat.textContent = 'Coût de cet appel ≈ ' + (j.coutUsd < 0.01 ? '<0,01' : j.coutUsd.toFixed(3)) + ' $' + (j.usage ? ('  (' + j.usage.input_tokens + '→' + j.usage.output_tokens + ' tokens)') : '');
        else etat.textContent = '';
      }
    }catch(e){ etat.textContent = 'Erreur de connexion au serveur local.'; historique.pop(); }
    btnEnvoyer.disabled = false; saisie.focus();
  }

  async function enregistrer(){
    if (!historique.length){ etat.textContent='Rien à enregistrer.'; return; }
    var titre = (historique[0].content||'conversation').slice(0,40);
    var md = '# Hermès — ' + new Date().toLocaleString('fr-FR') + '\n\n';
    historique.forEach(function(m){ md += (m.role==='user'?'## Moi\n':'## Hermès\n') + m.content + '\n\n'; });
    try{
      var r = await fetch('/api/save', { method:'POST', headers:{'Content-Type':'application/json'}, body: JSON.stringify({ titre:titre, contenu:md }) });
      var j = await r.json();
      etat.textContent = j.ok ? ('Enregistré : ' + j.fichier) : ('Échec : ' + (j.erreur||''));
    }catch(e){ etat.textContent='Échec de l\'enregistrement.'; }
  }

  btnEnvoyer.onclick = envoyer;
  document.getElementById('enregistrer').onclick = enregistrer;
  document.getElementById('effacer').onclick = function(){ historique=[]; conv.innerHTML=''; etat.textContent=''; var d=document.createElement('div'); d.className='vide'; d.innerHTML='<p style="font-size:2rem;margin:0">⚜️</p><p>Nouvelle conversation.</p>'; conv.appendChild(d); };
  saisie.addEventListener('keydown', function(e){ if(e.key==='Enter' && !e.shiftKey){ e.preventDefault(); envoyer(); } });
  saisie.addEventListener('input', function(){ saisie.style.height='auto'; saisie.style.height=Math.min(saisie.scrollHeight,160)+'px'; });
  chargerModeles();
</script>
</body>
</html>
````

### Étape 5 — Écrire `C:\Hermes\config.json` (voie cloud dormante)
Clé **vide** par défaut → aucun appel payant possible tant qu'on n'y met rien :

````json
{
  "_aide": "Hermes — configuration. AUJOURD'HUI tout est LOCAL (Ollama), gratuit. Pour activer une IA cloud plus tard : colle ta cle dans cloud.anthropic.cle puis relance Hermes. Tant que la cle est VIDE, le cloud reste desactive et aucun appel paye n'est possible.",
  "cloud": {
    "anthropic": {
      "cle": "",
      "modeles": ["claude-opus-4-8", "claude-sonnet-4-6", "claude-haiku-4-5"],
      "max_tokens": 4096
    }
  }
}
````

### Étape 6 — Écrire `C:\Hermes\Lancer-Hermes.cmd` (lance le chat local)
````bat
@echo off
rem Hermes local - demarre le serveur de chat Ollama et ouvre le navigateur.
title Hermes (local - Ollama)
"C:\Program Files\nodejs\node.exe" "C:\Hermes\chat\serveur.js"
````
> ⚠️ Si Node a été installé ailleurs, **adapte le chemin** vers `node.exe`. Pour le retrouver :
> `where.exe node` dans PowerShell.

### Étape 7 — Écrire `C:\Hermes\README.md` (la notice pour l'utilisateur)
````markdown
# Hermès — assistant local (Ollama)

Ton IA **locale, gratuite, hors ligne**. Aucun compte, aucun coût, rien ne sort de ton PC.

## Lancer
Double-clic sur le raccourci **Hermes-local** (bureau) → une **page de chat** s'ouvre dans ton navigateur.
*(Sinon : double-clic sur `Lancer-Hermes.cmd`.)*

## Comment ça marche
- Un petit serveur local (`chat\serveur.js`, Node) parle à **Ollama** installé sur ton PC.
- Choisis le **modèle** en haut à droite (généraliste, code, léger…).
- **💾 Enregistrer** : sauvegarde la conversation en `.md` dans `C:\Hermes\conversations\`.
- Pour **arrêter** : ferme la fenêtre « Hermes (local) » (c'est le serveur).

## Plus tard (optionnel) — brancher une IA cloud plus puissante
La voie cloud est **déjà câblée, mais dormante**. Pour l'activer avec une clé API :
1. Ouvre `C:\Hermes\config.json` et colle ta clé dans `cloud.anthropic.cle`.
2. Relance Hermès.

Les modèles **Claude** apparaissent alors dans le menu, marqués **« ☁ payant »**.
**Garde-fou facture** : chaque appel cloud **demande confirmation**, et le **coût estimé ($)**
s'affiche après. Tant que la clé est vide, le cloud reste **désactivé**.

*(Pour une IA agentique qui code en autonomie, c'est Claude Code — voir `CLAUDE.md`.)*
````

### Étape 8 — Créer les deux raccourcis bureau
Il faut **deux** raccourcis sur le Bureau :
- **Hermes** → lance **Claude Code** dans le dossier `C:\Hermes` (le super pouvoir agentique) ;
- **Hermes-local** → lance le chat Ollama (`Lancer-Hermes.cmd`).

Exécute ce script PowerShell. Il **retrouve tout seul** le `claude.exe` le plus récent et crée les deux raccourcis :

```powershell
$bureau = [Environment]::GetFolderPath('Desktop')
$ws = New-Object -ComObject WScript.Shell

# --- Raccourci 1 : Hermes (Claude Code agentique) ---
$claude = Get-ChildItem "$env:APPDATA\Claude\claude-code" -Recurse -Filter claude.exe -ErrorAction SilentlyContinue |
          Sort-Object LastWriteTime -Descending | Select-Object -First 1
if ($claude) {
  $sc = $ws.CreateShortcut("$bureau\Hermes.lnk")
  $sc.TargetPath = $claude.FullName
  $sc.WorkingDirectory = "C:\Hermes"
  $sc.WindowStyle = 1
  $sc.Description = "Hermes - Claude Code agentique (dossier C:\Hermes)"
  if (Test-Path "C:\Hermes\hermes-casque.ico") { $sc.IconLocation = "C:\Hermes\hermes-casque.ico" }
  $sc.Save()
  Write-Host "OK : raccourci Hermes -> $($claude.FullName)"
} else {
  Write-Host "ATTENTION : claude.exe introuvable. Lance Claude Code une fois depuis l'appli, puis relance ce script."
}

# --- Raccourci 2 : Hermes-local (chat Ollama) ---
$sc2 = $ws.CreateShortcut("$bureau\Hermes-local.lnk")
$sc2.TargetPath = "C:\Hermes\Lancer-Hermes.cmd"
$sc2.WorkingDirectory = "C:\Hermes"
$sc2.WindowStyle = 1
$sc2.Description = "Hermes local - chat Ollama hors ligne"
if (Test-Path "C:\Hermes\hermes-casque.ico") { $sc2.IconLocation = "C:\Hermes\hermes-casque.ico" }
$sc2.Save()
Write-Host "OK : raccourci Hermes-local"
```
> 💡 **Le chemin de `claude.exe` est « versionné »** : à chaque mise à jour de Claude Code, il change.
> Si le raccourci **Hermes** ne marche plus un jour, **relance simplement ce script PowerShell** —
> il re-pointera sur la nouvelle version.

### Étape 9 — Vérifier que tout marche
1. **Chat local** : double-clic sur **Hermes-local** → une fenêtre noire s'ouvre + le navigateur affiche la page Hermès sur `http://127.0.0.1:4545`. Choisis un modèle, écris « bonjour », vérifie la réponse. *(Si « Aucun modèle local » : Ollama n'est pas lancé ou aucun modèle n'a été téléchargé — voir PARTIE A étape 4.)*
2. **Super pouvoir** : double-clic sur **Hermes** → Claude Code s'ouvre dans `C:\Hermes`. Demande-lui par exemple de créer un fichier test dans `travaux\` pour confirmer qu'il a bien accès aux fichiers.
3. Annonce à l'utilisateur que c'est prêt, et rappelle-lui la règle d'or : **🛡️ données sensibles = on ne les envoie pas au cloud sans accord ; 🪽 Hermès = espace libre pour le reste.**

---

## Annexe — Icône (facultatif)
L'install originale utilise une petite **icône casque ailé** (`hermes-casque.ico`). Elle est **optionnelle** :
les raccourcis fonctionnent sans. Pour l'avoir, [PRÉNOM] peut copier le fichier `hermes-casque.ico`
depuis le PC d'origine (clé USB) dans `C:\Hermes\`, **avant** l'étape 8 — le script l'utilisera automatiquement.

## Annexe — Raccourci alternatif (si la copie manuelle est plus simple)
Plutôt que de tout recréer, on peut **copier le dossier `C:\Hermes` d'origine** sur une clé USB
(en **excluant** `conversations\` et `travaux\` qui sont personnels), le coller sur le nouveau PC,
puis ne faire que : adapter `CLAUDE.md` (prénom), et exécuter **l'étape 8** (raccourcis). Le résultat est identique.
