# Récupérer des données sur une API

Pour symplifier les requêtes, on installe la dépendance suivante :
`npm i axios`

dans `src` créer le fichier `axios.js` avec le contenu suivant :
```Javascript
import axios from 'axios'

const instance = axios.create({
	baseUrl: 'http://localhost:8001' //A adapter selon l'url de l'API
})

export default instance
```

Ensuite, pour lire les données, dans le composant réact :

en tête de fichier, ajouter :
```Javascript
import axios from './axios'
```
Dans le composant, utiliser useState et useEffect pour récupérer les données et les sauvegarder dans l'état :

```Javascript
const [people, usePeople]= useState([])

useEffect(() => {
	async function fetchData() {
		const req = await axios.get('/tinder/cards') //Selon requête
		setPeople(req.data)
	}
	fetchData()
}, [])
```