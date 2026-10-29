import random

# Configurações globais do Servidor
MAX_PLAYERS = 500
servidor_online = True

class ServidorRP:
    def __init__(self):
        self.jogadores_conectados = 0

    def entrar_servidor(self, jogador):
        if self.jogadores_conectados < MAX_PLAYERS:
            self.jogadores_conectados += 1
            print(f"✅ {jogador.nome} entrou no servidor. ({self.jogadores_conectados}/{MAX_PLAYERS} jogadores)")
        else:
            print("❌ Servidor lotado! (Máximo de 500 jogadores atingido).")

class Jogador:
    def __init__(self, nome):
        self.nome = nome
        self.face = 1 # 1 a 7
        self.cor_pele = "Preto" # Preto, Branco, Moreno
        self.cabelo = 1 # 1 a 12
        
        # Economia (Dinheiro na mão vs Banco Físico)
        self.dinheiro_limpo = 0
        self.dinheiro_sujo = 0
        self.banco = 0
        self.celular = False
        self.numero_celular = None
        
        # Localização no mapa (Simulação para ir ao banco físico)
        self.localizacao_atual = "Rua" 

    def criar_aparencia(self, face, pele, cabelo):
        if 1 <= face <= 7: self.face = face
        if pele in ["Preto", "Branco", "Moreno"]: self.cor_pele = pele
        if 1 <= cabelo <= 12: self.cabelo = cabelo
        print(f"👤 Personagem {self.nome} criado! [Pele: {self.cor_pele} | Face: {self.face} | Cabelo: {self.cabelo}]")

    def comprar_celular(self):
        if self.dinheiro_limpo >= 1000:
            self.dinheiro_limpo -= 1000
            self.celular = True
            self.numero_celular = f"21-9{random.randint(10000000, 99999999)}"
            print(f"📱 Celular comprado! Seu número é: {self.numero_celular}")
        else:
            print("❌ Dinheiro insuficiente para comprar celular (Custo: R$ 1.000).")

    def trabalhar_repositor(self):
        # Repositor no Mercado Rio Sul (Sem CNH) - Ajustado para R$ 100 por caixa
        pagamento = 100
        self.dinheiro_limpo += pagamento
        print(f"📦 Você pegou a caixa no 2º andar do Rio Sul e repôs a prateleira. Ganhou R$ {pagamento:.2f}!")

    def ir_ao_banco_fisico(self, valor):
        # O depósito agora exige ir fisicamente ao banco do mapa
        self.localizacao_atual = "Banco Central do Mapa"
        if self.dinheiro_limpo >= valor:
            self.dinheiro_limpo -= valor
            self.banco += valor
            print(f"🏦 Você foi até o **Banco Físico** e depositou R$ {valor:.2f}. Saldo no banco: R$ {self.banco:.2f}")
        else:
            print("❌ Você não tem esse valor em dinheiro limpo na mão para depositar!")

    def mostrar_hud(self):
        print("\n" + "="*35)
        print(f"👤 Player: {self.nome} | Local: {self.localizacao_atual}")
        print(f"💵 Dinheiro Limpo (Mão)    : R$ {self.dinheiro_limpo:.2f}")
        print(f"🔴 Dinheiro Sujo (Mão)    : R$ {self.dinheiro_sujo:.2f}")
        print(f"🏦 Saldo no Banco (Físico) : R$ {self.banco:.2f}")
        print("="*35 + "\n")

# --- TESTANDO AS REGRAS CORRIGIDAS ---
cidade_rp = ServidorRP()

p1 = Jogador("Breno")
cidade_rp.entrar_servidor(p1)
p1.criar_aparencia(face=3, pele="Moreno", cabelo=8)

# Trabalhando 3 vezes no Rio Sul (R$ 100 cada)
p1.trabalhar_repositor()
p1.trabalhar_repositor()
p1.trabalhar_repositor()
p1.mostrar_hud()

# Tentando depositar indo ao banco físico
p1.ir_ao_banco_fisico(250)
p1.mostrar_hud()

