- 👋 Hi, I’m @Sizomk
- 👀 I’m interested in ...
- 🌱 I’m currently learning ...
- 💞️ I’m looking to collaborate on ...
- 📫 How to reach me ...
- 😄 Pronouns: ...
- ⚡ Fun fact: ...
- import pygame
import random

# Inicializa o Pygame
pygame.init()

# Define as cores
BRANCO = (255, 255, 255)
PRETO = (0, 0, 0)
VERMELHO = (255, 0, 0)
AZUL = (0, 0, 255)

# Dimensões da tela
LARGURA_TELA = 400
ALTURA_TELA = 600
tela = pygame.display.set_mode((LARGURA_TELA, ALTURA_TELA))
pygame.display.set_caption("Game de Corrida")

# Definições do carro
largura_carro = 50
altura_carro = 100
pos_x_carro = LARGURA_TELA // 2 - largura_carro // 2
pos_y_carro = ALTURA_TELA - altura_carro - 20

# Definições do obstáculo
largura_obstaculo = 50
altura_obstaculo = 100
velocidade_obstaculo = 7
pos_x_obstaculo = random.randint(0, LARGURA_TELA - largura_obstaculo)
pos_y_obstaculo = -altura_obstaculo

# Definições de movimento
velocidade_carro = 7
direcao_x_carro = 0

# Configuração de pontuação
pontuacao = 0
fonte_pontuacao = pygame.font.SysFont(None, 40)

# Função para desenhar o carro
def desenhar_carro(x, y):
    pygame.draw.rect(tela, AZUL, [x, y, largura_carro, altura_carro])

# Função para desenhar o obstáculo
def desenhar_obstaculo(x, y):
    pygame.draw.rect(tela, VERMELHO, [x, y, largura_obstaculo, altura_obstaculo])

# Função para mostrar a pontuação
def mostrar_pontuacao(pontos):
    texto = fonte_pontuacao.render(f"Pontos: {pontos}", True, PRETO)
    tela.blit(texto, [10, 10])

# Função principal do jogo
def game():
    global pos_x_carro, pos_y_carro, direcao_x_carro, pos_x_obstaculo, pos_y_obstaculo, pontuacao

    # Variável para controlar o loop do jogo
    jogo_rodando = True

    # Clock do jogo
    clock = pygame.time.Clock()

    while jogo_rodando:
        for evento in pygame.event.get():
            if evento.type == pygame.QUIT:
                jogo_rodando = False

            # Controle do carro com as setas do teclado
            if evento.type == pygame.KEYDOWN:
                if evento.key == pygame.K_LEFT:
                    direcao_x_carro = -velocidade_carro
                elif evento.key == pygame.K_RIGHT:
                    direcao_x_carro = velocidade_carro

            if evento.type == pygame.KEYUP:
                if evento.key == pygame.K_LEFT or evento.key == pygame.K_RIGHT:
                    direcao_x_carro = 0

        # Atualiza a posição do carro
        pos_x_carro += direcao_x_carro

        # Mantém o carro dentro dos limites da tela
        if pos_x_carro < 0:
            pos_x_carro = 0
        elif pos_x_carro > LARGURA_TELA - largura_carro:
            pos_x_carro = LARGURA_TELA - largura_carro

        # Movimenta o obstáculo
        pos_y_obstaculo += velocidade_obstaculo

        # Reposiciona o obstáculo quando sai da tela e aumenta a pontuação
        if pos_y_obstaculo > ALTURA_TELA:
            pos_y_obstaculo = -altura_obstaculo
            pos_x_obstaculo = random.randint(0, LARGURA_TELA - largura_obstaculo)
            pontuacao += 1

            # Aumenta a dificuldade (velocidade do obstáculo) conforme a pontuação
            if pontuacao % 5 == 0:
                velocidade_obstaculo += 1

        # Verifica colisão
        if (pos_y_carro < pos_y_obstaculo + altura_obstaculo and
            pos_y_carro + altura_carro > pos_y_obstaculo and
            pos_x_carro < pos_x_obstaculo + largura_obstaculo and
            pos_x_carro + largura_carro > pos_x_obstaculo):
            jogo_rodando = False  # Fim de jogo em caso de colisão

        # Preenche o fundo da tela
        tela.fill(BRANCO)

        # Desenha o carro e o obstáculo
        desenhar_carro(pos_x_carro, pos_y_carro)
        desenhar_obstaculo(pos_x_obstaculo, pos_y_obstaculo)

        # Mostra a pontuação
        mostrar_pontuacao(pontuacao)

        # Atualiza a tela
        pygame.display.update()

        # Controla a taxa de frames por segundo
        clock.tick(60)

    # Exibe a tela final com a pontuação
    tela.fill(BRANCO)
    texto_final = fonte_pontuacao.render(f"Fim de Jogo! Pontuação final: {pontuacao}", True, PRETO)
    tela.blit(texto_final, [LARGURA_TELA // 6, ALTURA_TELA // 2])
    pygame.display.update()
    pygame.time.wait(3000)  # Espera 3 segundos antes de fechar o jogo

    pygame.quit()

# Inicia o jogo
game()


<!---
Sizomk/Sizomk is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
