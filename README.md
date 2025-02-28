import random
import re
from validate_docbr import CPF
from email_validator import validate_email, EmailNotValidError
from datetime import datetime

class ValidadorAntifraude:
    def __init__(self):
        self.cpf_validator = CPF()

    def validar_cpf(self, cpf):
        if not self.cpf_validator.validate(cpf):
            raise ValueError("CPF inválido.")

    def validar_telefone(self, telefone):
        telefone_regex = re.compile(r"^\d{10,11}$")
        if not telefone_regex.match(telefone):
            raise ValueError("Telefone inválido.")

    def validar_email(self, email):
        try:
            validate_email(email)
        except EmailNotValidError:
            raise ValueError("Email inválido.")

    def validar_data_nascimento(self, data_nascimento):
        try:
            datetime.strptime(data_nascimento, '%d/%m/%Y')
        except ValueError:
            raise ValueError("Data de nascimento inválida.")

    def validar_nome_completo(self, nome):
        if not nome or len(nome.split()) < 2:
            raise ValueError("Nome completo inválido.")

    def validar_endereco(self, endereco):
        if not endereco:
            raise ValueError("Endereço inválido.")

    def validar_nome_mae(self, nome_mae):
        if not nome_mae:
            raise ValueError("Nome da mãe inválido.")

    def validar_dados(self, dados):
        try:
            self.validar_cpf(dados["cpf"])
            self.validar_telefone(dados["telefone"])
            self.validar_email(dados["email"])
            self.validar_data_nascimento(dados["data_nascimento"])
            self.validar_nome_completo(dados["nome"])
            self.validar_endereco(dados["endereco"])
            self.validar_nome_mae(dados["nome_mae"])
        except ValueError:
            return 0  # Dados inválidos, grau de confiabilidade zero

        return random.randint(0, 10)  # Dados válidos, gera um grau de confiabilidade aleatório

# Exemplo de uso:
if __name__ == "__main__":
    validador = ValidadorAntifraude()
    exemplo_dados = {
        "cpf": "12345678901",
        "nome": "Fulano de Tal",
        "telefone": "11999999999",
        "email": "fulano@email.com",
        "data_nascimento": "01/01/1990",
        "endereco": "Rua Exemplo, 123",
        "nome_mae": "Maria da Silva"
    }

    grau_confiabilidade = validador.validar_dados(exemplo_dados)
    print(f"Grau de Confiabilidade: {grau_confiabilidade}")

    # Exemplo com dados inválidos:
    exemplo_dados_invalido = {
        "cpf": "12345678902",  # CPF inválido
        "nome": "Fulano",      # Nome incompleto
        "telefone": "11999999999",
        "email": "fulano@email.com",
        "data_nascimento": "01/01/1990",
        "endereco": "Rua Exemplo, 123",
        "nome_mae": "Maria da Silva"
    }

    grau_confiabilidade_invalido = validador.validar_dados(exemplo_dados_invalido)
    print(f"Grau de Confiabilidade (inválido): {grau_confiabilidade_invalido}")
