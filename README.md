# Dio_Bank
Projeto que visa criar um banco....
import os

def limpar_tela():
    os.system('cls' if os.name == 'nt' else 'clear')

def mostrar_boas_vindas():
    limpar_tela()
    print("""
    =======================================
               Bem-vindo ao Dio Bank
    =======================================
    """)
    input("Aperte qualquer tecla para continuar...")

Dio_Bank= "Dio Bank" 

mostrar_boas_vindas()
limpar_tela()

print(f"Bem vindo ao {Dio_Bank}!")

Menu = """
[d] Depositar
[s] Sacar
[e] Extrato
[q] Sair

=> """

saldo = 0
limite = 500
extrato = ""
numero_saques = 0
LIMITE_SAQUES = 3

def confirmar_operacao(mensagem):
    while True:
        confirmacao = input(f"{mensagem} (S/N): ").upper()
        if confirmacao in ("S", "N"):
            return confirmacao == "S"
        else:
            print("Opção inválida. Digite S ou N.")

def depositar(saldo: float, valor: float) -> tuple[float, str]:
    if valor <= 0:
        raise ValueError("Valor do depósito deve ser positivo.")

    if confirmar_operacao(f"Confirmar depósito de R$ {valor:.2f}?"):
        saldo += valor
        extrato = f"Depósito: R$ {valor:.2f}\n"
        print("Depósito realizado com sucesso!")
        return saldo, extrato
    else:
        print("Depósito cancelado.")
        return saldo, ""

def sacar(saldo: float, valor: float, extrato: str, limite: float, numero_saques: int, LIMITE_SAQUES: int) -> tuple[float, str, int]:
    if valor <= 0:
        raise ValueError("Valor do saque deve ser positivo.")

    if valor > saldo:
        raise ValueError("Saldo insuficiente para saque.")

    if valor > limite:
        raise ValueError("Valor do saque excede o limite.")

    if numero_saques >= LIMITE_SAQUES:
        raise ValueError("Número máximo de saques diários excedido.")

    if confirmar_operacao(f"Confirmar saque de R$ {valor:.2f}?"):
        saldo -= valor
        extrato = f"Saque: R$ {valor:.2f}\n"
        numero_saques += 1
        print("Saque realizado com sucesso!")
        return saldo, extrato, numero_saques
    else:
        print("Saque cancelado.")
        return saldo, "", numero_saques

while True:
    opcao = input(Menu)

    if opcao == "d":
        valor = float(input("Informe o valor do depósito: "))
        try:
            saldo, extrato_atual = depositar(saldo, valor)
            extrato += extrato_atual
        except ValueError as e:
            print(f"Erro: {e}")

    elif opcao == "s":
        valor = float(input("Informe o valor do saque: "))
        try:
            saldo, extrato_atual, numero_saques = sacar(saldo, valor, extrato, limite, numero_saques, LIMITE_SAQUES)
            extrato += extrato_atual
        except ValueError as e:
            print(f"Erro: {e}")

    elif opcao == "e":
        print("\n================ EXTRATO ================\n")
        print("Não foram realizadas movimentações." if not extrato else extrato)
        print(f"\nSaldo: R$ {saldo:.2f}")
        print("==========================================")

    elif opcao == "q":
        print("Obrigado por usar o Dio Bank!")
        break

    else:
        print("Operação inválida, por favor selecione novamente a operação desejada.")
