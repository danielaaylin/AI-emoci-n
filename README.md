from flask import Flask, request, jsonify
import random

app = Flask(__name__)

# Emociones simuladas y cómo las combinamos
emociones = ["tristeza", "felicidad", "ansiedad", "gratitud", "ira", "sorpresa"]

def analizar_emocion(texto):
    # Simula la detección de emociones y combinaciones
    emocion_principal = random.choice(emociones)
    emocion_secundaria = random.choice([e for e in emociones if e != emocion_principal])
    return {"primaria": emocion_principal, "secundaria": emocion_secundaria}

@app.route("/analizar", methods=["POST"])
def analizar():
    data = request.json
    texto = data.get("texto", "")

    if not texto:
        return jsonify({"error": "No se proporcionó texto para analizar"}), 400

    resultado = analizar_emocion(texto)
    return jsonify(resultado)

@app.route("/tutor-notificacion", methods=["POST"])
def notificar_tutor():
    data = request.json
    emocion = data.get("emocion", "")
    tutor = data.get("tutor", "")

    # Simula el envío de una alerta al tutor
    if emocion == "tristeza":
        mensaje = f"Alerta: Detectamos tristeza prolongada. Recomendamos contactar a {tutor}."
        return jsonify({"mensaje": mensaje}), 200
    else:
        return jsonify({"mensaje": "No se requiere acción inmediata"}), 200

if __name__ == "__main__":
    app.run(debug=True)
