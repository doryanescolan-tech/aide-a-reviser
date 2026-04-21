# aide-a-reviser
il ais super bugger je vous invite a le modifier
<!DOCTYPE html>
<html lang="fr">
<head>
<meta charset="UTF-8">
<title>Révision Avancée</title>

<style>
body { font-family: Arial; padding: 20px; }
#zoneTexte { border: 1px solid black; padding: 10px; min-height: 100px; }
.trou { background: #ddd; padding: 2px; }
input.reponse { width: 100px; margin: 2px; }
</style>

</head>
<body>

<h1>🧠 Révision intelligente</h1>

<div id="zoneTexte" contenteditable="true">
Tape ton texte ici...
</div>

<br>

<button onclick="creerTrou()">Créer un trou</button>
<button onclick="sauvegarder()">💾 Sauvegarder</button>
<button onclick="charger()">📂 Charger</button>

<br><br>

<label>Temps (secondes) :</label>
<input type="number" id="temps" value="5">

<button onclick="lancer()">Lancer le quiz</button>
<button onclick="afficherUneReponse()">👁 Afficher 1 réponse</button>

<p id="compte"></p>

<script>
let reponses = [];

function creerTrou() {
    let selection = window.getSelection();
    let texte = selection.toString();

    if (texte.length > 0) {
        let input = document.createElement("input");
        input.className = "reponse";
        input.dataset.reponse = texte;

        reponses.push(input);

        let range = selection.getRangeAt(0);
        range.deleteContents();
        range.insertNode(input);
    }
}

function lancer() {
    let temps = document.getElementById("temps").value;
    let compte = document.getElementById("compte");

    compte.innerText = "⏳ " + temps + " secondes... mémorise !";

    // cacher les réponses temporairement
    reponses.forEach(input => {
        input.placeholder = "_____";
        input.value = "";
    });

    let interval = setInterval(() => {
        temps--;
        compte.innerText = "⏳ " + temps + " secondes...";

        if (temps <= 0) {
            clearInterval(interval);
            compte.innerText = "✏️ Remplis les trous !";
        }
    }, 1000);
}

function afficherUneReponse() {
    for (let input of reponses) {
        if (input.value === "") {
            input.value = input.dataset.reponse;
            break;
        }
    }
}

function sauvegarder() {
    localStorage.setItem("texte", document.getElementById("zoneTexte").innerHTML);
    alert("✅ Sauvegardé !");
}

function charger() {
    let contenu = localStorage.getItem("texte");
    if (contenu) {
        document.getElementById("zoneTexte").innerHTML = contenu;

        // reconstruire les inputs
        reponses = Array.from(document.querySelectorAll("input.reponse"));
    }
}
</script>

</body>
</html>
