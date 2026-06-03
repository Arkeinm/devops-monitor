# 🚀 DevOps Monitoring Dashboard — MVP

Ce mini‑projet présente un **dashboard de monitoring temps réel** avec :

- ⚙️ **Backend FastAPI** : collecte et exposition des métriques, gestion des serveurs monitorés
- 📊 **Frontend Streamlit** : affichage dynamique des données
- 🧪 **Tests pytest** : couverture et vérification des fonctionnalités

---

## 📋 Prérequis

- Python **≥ 3.10** *(testé sur Python 3.13)*
- `pip`
- Environnement virtuel recommandé

---

## ⚙️ Installation

```bash
python -m venv .venv
source .venv/bin/activate   # Windows : .venv\Scripts\activate
pip install -r requirements.txt
```

---

## 🔧 Configuration

Une clé API est nécessaire pour les opérations d’écriture (`POST`, `DELETE`).

```bash
export API_KEY=dev-secret-key
```

Un paramètre par défaut est prévu pour le développement local.

---

## ▶ Démarrage

### Backend FastAPI

```bash
uvicorn api.main:app --reload --port 8000
```

API disponible :

```bash
http://localhost:8000
```

Documentation interactive :

```bash
http://localhost:8000/docs
```

### Frontend Streamlit

Dans un autre terminal :

```bash
streamlit run dashboard/app.py
```

Interface disponible :

```bash
http://localhost:8501
```

---

## ✅ Tests

### Tests unitaires et fonctionnels

```bash
pytest tests/ -v
```

Couvre notamment :

- `GET /health`
- `GET /metrics`
- CRUD des serveurs
- validation de la clé API
- vérification de `GET /servers/{id}/check`

### Couverture de code

```bash
pytest --cov=api tests/ -v
```

Objectifs :

- tous les tests doivent réussir
- couverture minimale attendue : **75 %**

### Tests manuels (optionnels)

#### Vérification du service

```bash
curl http://localhost:8000/health
```

#### Récupération des métriques

```bash
curl http://localhost:8000/metrics
```

#### Ajout d’un serveur

```bash
curl -X POST http://localhost:8000/servers \
  -H "X-API-Key: dev-secret-key" \
  -H "Content-Type: application/json" \
  -d '{"name":"local","host":"localhost","port":8000}'
```

### Test WebSocket

Le Swagger ne montre pas toujours les WebSockets ; utilisez un client Python pour vérifier.

```bash
pip install websockets
```

```python
# ws_test.py
import asyncio
import websockets

async def test_ws():
    async with websockets.connect("ws://localhost:8000/ws/metrics") as ws:
        for _ in range(3):
            print(await ws.recv())

asyncio.run(test_ws())
```

```bash
python ws_test.py
```

Si vous recevez un JSON chaque seconde, le WebSocket fonctionne correctement.



