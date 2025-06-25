import json
import os
from difflib import get_close_matches
 
ARQUIVO_FAQ = "faq.json"
 
# Carrega as perguntas e respostas do arquivo, se existir
def carregar_faq():
    if os.path.exists(ARQUIVO_FAQ):
        with open(ARQUIVO_FAQ, "r", encoding="utf-8") as f:
            return json.load(f)
    return []
 
# Salva a base de dados no arquivo JSON
def salvar_faq(faq):
    with open(ARQUIVO_FAQ, "w", encoding="utf-8") as f:
        json.dump(faq, f, ensure_ascii=False, indent=2)
 
# Tenta encontrar uma resposta por palavra-chave ou similaridade
def encontrar_resposta(pergunta, faq):
    pergunta_lower = pergunta.lower()
    palavras = pergunta_lower.split()
 
    # Procura por correspondência exata ou palavras-chave
    for item in faq:
        if any(p in pergunta_lower for p in item.get("palavras_chave", [])):
            return item["resposta"]
 
    # (Opcional) Usa similaridade de texto para sugerir perguntas parecidas
    perguntas_existentes = [item["pergunta"] for item in faq]
    similares = get_close_matches(pergunta, perguntas_existentes, n=1, cutoff=0.6)
    if similares:
        for item in faq:
            if item["pergunta"] == similares[0]:
                print(f"\nPergunta parecida encontrada: {item['pergunta']}")
                return item["resposta"]
 
    return None
 
# Loop principal do terminal
def main():
    faq = carregar_faq()
    print("Sistema de Perguntas e Respostas. Digite 'sair' para encerrar.\n")
 
    while True:
        pergunta = input("Usuário > ").strip()
        if pergunta.lower() == "sair":
            break
 
        resposta = encontrar_resposta(pergunta, faq)
        if resposta:
            print("Resposta:", resposta)
        else:
            print("Essa pergunta ainda não tem resposta.")
            resp = input("Deseja adicionar uma resposta agora? (s/n) > ").strip().lower()
            if resp == "s":
                nova_resposta = input("Digite a resposta > ")
                palavras_chave = input("Digite palavras-chave separadas por vírgula > ").split(",")
                faq.append({
                    "pergunta": pergunta,
                    "resposta": nova_resposta,
                    "palavras_chave": [p.strip().lower() for p in palavras_chave]
                })
                salvar_faq(faq)
                print("Resposta salva com sucesso!")
 
if __name__ == "__main__":
    main()## Hi there 👋

<!--
**victorgabrielmedina/victorgabrielmedina** is a ✨ _special_ ✨ repository because its `README.md` (this file) appears on your GitHub profile.

Here are some ideas to get you started:

- 🔭 I’m currently working on ...
- 🌱 I’m currently learning ...
- 👯 I’m looking to collaborate on ...
- 🤔 I’m looking for help with ...
- 💬 Ask me about ...
- 📫 How to reach me: ...
- 😄 Pronouns: ...
- ⚡ Fun fact: ...
-->
