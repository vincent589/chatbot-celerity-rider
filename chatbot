from flask import Flask, request, jsonify
import webbrowser

app = Flask(__name__)

# Base de données fictive des produits (pour l'instant)
PRODUCTS = [
    {"name": "Pull SpeedRider", "category": "pull", "style": "sobre", "price": 39.99},
    {"name": "T-shirt RiderX", "category": "t-shirt", "style": "sportif", "price": 24.99},
    {"name": "Blouson Storm", "category": "blouson", "style": "hiver", "price": 89.99},
    {"name": "Gants Moto Fury", "category": "gants", "style": "sportif", "price": 29.99},
]

# Questions fréquentes
FAQ = {
    "livraison": "Nos délais de livraison sont de 3 à 5 jours ouvrés.",
    "retour": "Vous avez 14 jours pour effectuer un retour après réception de votre commande.",
    "paiement": "Nous acceptons les paiements par carte bancaire, PayPal et virement bancaire.",
    "taille": "Pour vous aider, nous proposons un guide des tailles sur chaque page produit.",
    "contact": "Vous pouvez contacter notre service client au 06 XX XX XX XX ou par email à contact@celerityrider.fr."
}

# Fonction pour répondre aux questions classiques
def get_faq_response(question):
    for key in FAQ:
        if key in question.lower():
            return FAQ[key]
    if "humain" in question:
        return "Je vous invite à consulter notre page de contact : [Lien vers la page contact](https://celerityrider.fr/contact) ou à nous écrire à contact@celerityrider.fr"
    return "Je ne suis pas sûr de comprendre. Souhaitez-vous parler à un humain ?"

# Fonction pour recommander un produit
def recommend_product(request):
    category = request.get("category", "").lower()
    style = request.get("style", "").lower()

    recommendations = [
        product for product in PRODUCTS
        if (category in product["category"]) or (style in product["style"])
    ]

    if recommendations:
        response = "Je vous recommande :\n"
        for product in recommendations:
            response += f"- {product['name']} ({product['price']} €)\n"
        return response.strip()
    else:
        return "Je n'ai pas trouvé de produit correspondant à votre recherche."

@app.route('/chatbot', methods=['POST'])
def chatbot():
    data = request.json.get("message", "").lower()

    # Message de bienvenue
    if data in ["bonjour", "salut", "hello"]:
        return jsonify({
            "response": "Bienvenue chez Célérity Rider ! Je suis Celerity, le bot pour répondre à vos questions classiques et vous aider à trouver des articles intéressants. Si je ne peux pas répondre, je vous redirigerai vers notre service client."
        })

    # Vérifie les questions classiques (FAQ)
    faq_response = get_faq_response(data)
    if faq_response != "Je ne suis pas sûr de comprendre. Souhaitez-vous parler à un humain ?":
        return jsonify({"response": faq_response})

    # Vérifie les demandes de recommandation produit
    if "conseil" in data or "recommande" in data or "pull" in data or "t-shirt" in data:
        return jsonify({"response": recommend_product(request.json)})

    # Réponse par défaut
    return jsonify({
        "response": "Je suis là pour vous aider ! Vous avez une question précise sur nos produits ou services ?"
    })