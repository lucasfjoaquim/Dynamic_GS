# Otimização de Pontos em Espaço Tridimensional e Visualização de Vetores de Movimento

Este projeto contém funções para gerar pontos otimizados em um espaço tridimensional e visualizar os vetores de movimento de um conjunto de pontos iniciais para um conjunto de pontos finais. Essas funções podem ser usadas para diversas aplicações, incluindo o cálculo de rotas de sensores autônomos para migrarem de uma área para outra.

## Funções Disponíveis

### 1. `gerar_pontos_otimizados(canto1, canto2, n)`

Gera N pontos otimizados em um espaço tridimensional definido por duas coordenadas opostas.

#### Parâmetros:
- `canto1` (tuple): Coordenadas de um canto do espaço (x1, y1, z1).
- `canto2` (tuple): Coordenadas do canto oposto do espaço (x2, y2, z2).
- `n` (int): Número de pontos a serem gerados.

#### Retorna:
- `list`: Lista de tuplas representando os pontos otimizados (x, y, z).

### 2. `plotar_vetores_movimento(pontos_A, pontos_B)`

Plota os vetores de movimento de pontos de A para B em um gráfico 3D.

#### Parâmetros:
- `pontos_A` (list): Lista de tuplas representando os pontos iniciais (x, y, z).
- `pontos_B` (list): Lista de tuplas representando os pontos finais (x, y, z).

## Exemplo de Uso

### Geração e Visualização de Pontos Otimizados

```python
import numpy as np
import matplotlib.pyplot as plt
from mpl_toolkits.mplot3d import Axes3D

def gerar_pontos_otimizados(canto1, canto2, n):
    x1, y1, z1 = canto1
    x2, y2, z2 = canto2

    num_pontos_por_dimensao = int(round(n ** (1/3)))
    
    x = np.linspace(x1, x2, num_pontos_por_dimensao)
    y = np.linspace(y1, y2, num_pontos_por_dimensao)
    z = np.linspace(z1, z2, num_pontos_por_dimensao)

    pontos = [(xi, yi, zi) for xi in x for yi in y for zi in z]

    if len(pontos) > n:
        pontos = pontos[:n]

    return pontos

def plotar_vetores_movimento(pontos_A, pontos_B):
    fig = plt.figure()
    ax = fig.add_subplot(111, projection='3d')

    xA, yA, zA = zip(*pontos_A)
    xB, yB, zB = zip(*pontos_B)

    ax.scatter(xA, yA, zA, c='r', marker='o', label='Pontos A')
    ax.scatter(xB, yB, zB, c='b', marker='^', label='Pontos B')

    for i in range(len(pontos_A)):
        ax.quiver(xA[i], yA[i], zA[i], xB[i]-xA[i], yB[i]-yA[i], zB[i]-zA[i], color='g')

    ax.set_xlabel('Eixo X')
    ax.set_ylabel('Eixo Y')
    ax.set_zlabel('Eixo Z')
    ax.legend()
    plt.show()

canto1_A = (0, 0, 0)
canto2_A = (10, 10, 10)
canto1_B = (5, 5, 5)
canto2_B = (15, 15, 15)
n = 8

pontos_A = gerar_pontos_otimizados(canto1_A, canto2_A, n)
pontos_B = gerar_pontos_otimizados(canto1_B, canto2_B, n)
print(f"Pontos A: {pontos_A}")
print(f"Pontos B: {pontos_B}")

plotar_vetores_movimento(pontos_A, pontos_B)