import random

jogador = {
    "vida": 300,
    "ataque": 20,
    "pocoes": 2,
    "pontos": 0,
    "derrotados": 0
}

tipos = [
    {"nome": "Goblin", "vida": 40, "ataque": 8, "pontos": 100},
    {"nome": "Orc", "vida": 55, "ataque": 11, "pontos": 200},
    {"nome": "Troll", "vida": 70, "ataque": 14, "pontos": 300},
    {"nome": "Beast", "vida": 85, "ataque": 17, "pontos": 400},
    {"nome": "Dragao", "vida": 100, "ataque": 20, "pontos": 500}
]

# escolher 3 a 5 inimigos ao acaso
inimigos = []
quantidade = random.randint(3, 5)
for i in range(quantidade):
    escolhido = random.choice(tipos)
    # criar um inimigo novo, para não partilhar a vida
    inimigos.append({
        "nome": escolhido["nome"],
        "vida": escolhido["vida"],
        "ataque": escolhido["ataque"],
        "pontos": escolhido["pontos"]
    })

print("========================================")
print("          BATALHA DE MONSTROS           ")
print("========================================")

resultado = ""

while True:
    print("\n========== BATALHA ==========")
    print("1 - Atacar inimigo")
    print("2 - Consultar estado do jogador")
    print("3 - Listar inimigos vivos")
    print("4 - Usar pocao de cura")
    print("5 - Fugir / terminar jogo")
    print("=============================")

    opcao = input("Escolha: ")

 # 1 - Atacar inimigo:

    if opcao == "1":
        vivos = []
        for inimigo in inimigos:
            if inimigo["vida"] > 0:
                vivos.append(inimigo)
        if(len(vivos) == 0 ):
            print ("Não existem inimigos vivos para atacar.")
        else:
            print("Inimigos vivos:")
            for i in range(len(vivos)): 
                print(str(i + 1) + " - " + vivos[i]["nome"] + " | Vida: " + str(vivos[i]["vida"]) + " | Ataque: " + str(vivos[i]["ataque"]))


            escolha = input("Escolha o inimigo: ")
            if escolha.isdigit() == False:
                print("Escreve um numero!")
            else:
                numero = int(escolha)
                if numero < 1 or numero > len(vivos):
                    print("Escolha invalida!")
                else:
                    #inimigos se estiver vivo adcionar os 10 e 20 pontos 
                    inimigo = vivos[numero - 1]
                    dano = random.randint(10, 20)
                    inimigo["vida"] = inimigo["vida"] - dano
                    if inimigo["vida"] < 0:
                        inimigo["vida"] = 0
                        print("Atacaste o " + inimigo["nome"] + " e causaste " + str(dano) + " de dano.")

                    escolha = input("Escolha o inimigo: ")
                    if escolha.isdigit() == False:
                        print("Escreve um numero!")
                    else:
                        numero = int(escolha)
                    if numero < 1 or numero > len(vivos):
                        print("Escolha invalida!")
                    else:
                        inimigo = vivos[numero - 1]


                    # escolher um inimigo aleatoriamente 
                    dano = random.randint(10, 20)
                    inimigo["vida"] = inimigo["vida"] - dano
                    if inimigo["vida"] < 0:
                        inimigo["vida"] = 0
                    print("Atacaste o " + inimigo["nome"] + " e causaste " + str(dano) + " de dano.")

                    # inimigos se a vida for igual a , adiciono pontos
                    if inimigo["vida"] == 0:
                        jogador["pontos"] = jogador["pontos"] + inimigo["pontos"]
                        jogador["derrotados"] = jogador["derrotados"] + 1
                        print(inimigo["nome"] + " foi derrotado! +" + str(inimigo["pontos"]) + " pontos.")
                    # senão mair que 0, inimigo me contra ataca 
                    else: 
                        print(inimigo["nome"] + " ficou com " + str(inimigo["vida"]) + " de vida.")
                        contra = random.randint(5, inimigo["ataque"])
                        jogador["vida"] = jogador["vida"] - contra
                        if jogador["vida"] < 0:
                            jogador["vida"] = 0
                        print(inimigo["nome"] + " contra-atacou e causou " + str(contra) + " de dano.")
                        print("Ficaste com " + str(jogador["vida"]) + " de vida.")

# 2 - Consultar estado:
    elif opcao == "2":
        vivos = 0
        for inimigo in inimigos:
            if inimigo["vida"] > 0:
                vivos = vivos + 1
    print("\n--- Estado do jogador ---")
    print("Vida: " + str(jogador["vida"]))
    print("Pontos: " + str(jogador["pontos"]))
    print("Inimigos derrotados: " + str(jogador["derrotados"]))
    print("Inimigos vivos: " + str(vivos))
    print("Pocoes: " + str(jogador["pocoes"]))

    
# 3 - Listar inimigos vivos:          
    if opcao == "3":
        encontrou = False
        for i in range(len(inimigos)):
            if inimigos[i]["vida"] > 0:
                print(str(i + 1) + " - " + inimigos[i]["nome"] + " | Vida: " + str(inimigos[i]["vida"]) +
                       " | Ataque: " + str(inimigos[i]["ataque"]))
            encontrou = True
        if encontrou == False:
            print("Nao existem inimigos vivos.")
            print


# 4 - Usar poção de cura:
    elif opcao == "4":
        if jogador["pocoes"] <= 0:
            print("Nao tens pocoes.")
        elif jogador["vida"] >= 300:
            print("Ja tens a vida cheia. Nao precisas de pocao.")
        else:
            jogador["vida"] = jogador["vida"] + 25
            if jogador["vida"] > 300:
                jogador["vida"] = 300
            jogador["pocoes"] = jogador["pocoes"] - 1
            print("Pocao usada! Vida: " + str(jogador["vida"]) + " | Pocoes restantes: " + str(jogador["pocoes"]))


# 5- Fugir:
    if opcao == 5:
        print("Você fugiu da batalha")
        resultado = "FUGA"
        break


#Condições de fim do jogo
def terminar_jogo():
    if jogador["vida"] <= 0:
        resultado = "DERROTA"
    elif jogador["derrotados"] == len(inimigos):
        resultado = "VITÓRIA"

