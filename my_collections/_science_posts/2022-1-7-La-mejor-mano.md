---
layout: post
title: Análisis de "Dealing the best hand of cards"
description: Dado un deck de cartas, encontrar la mejor mano posible
image: images/sci/dealing_the_best_hand/dealing_the_best_hand.jpg
author: Al Andrade
---



Este problema se encuentra con dos soluciones en el libro "Programming Interview Problems: Dynamic Programming" de Leonardo Rossi.

Lo que hago en este notebook es graficar la eficiencia de ambas soluciones y hago una modificación en el código para que nos entregue la suma y el set de números que dieron ese resultado.


## Planteamiento

Dada una baraja de cartas, tenemos que repartir una mano con cierto número de cartas. Las cartas pueden ser repartidas de arriba o de abajo de la baraja. Determina la mejor mano que puede ser repartida en términos de la suma de valores de las cartas, asumiendo que cada carta tiene un valor específico.

Ejemplo: Para la baraja [3, 1, 1, 6, 2] podemos repartir las siguientes manos de 3 cartas: [3, 1, 1], [3, 1, 2], [3, 2, 6], [2, 6, 1]. La mejor mano es [3, 2, 6] con la suma 11.

Preguntas de aclaración:

- ¿El repartidor sabe de los valores de las cartas en la baraja y su orden?
    Si, asume un completo conocimiento de la baraja
- ¿Que tan grande puede ser una mano?
    El tamaño límite es 1,000,000 de cartas
- ¿La mano puede estar vacía?
    Si, en ese caso, el valor es 0
- ¿Qué pasa si no hay suficientes cartas en la baraja para construir una mano del tamaño requerido?
    En ese caso todo la baraja debe ser repartida.

Se repartirán las siguientes manos:

- Todas las cartas de abajo de la baraja
- n - 1 cartas de abajo y 1 de arriba
- n - 2 cartas de abajo y 2 de arriba
- ...
- Todas las cartas de arriba

## Solución 1: Fuerza bruta. Eficiencia O(n<sup>2</sup>)

Para esta solución, modifiqué el código presentado en el libro para que el programa no solo entregue la suma de la mejor mano, si no que también entregue la mano que da esa suma.

    import time
    from typing import List

    def get_best_hand_from_deck(deck: List, hand_size: int):
        '''
        :param deck: Lista de números que representan las cartas con valor númerico
        :param hand_size: número entero, el tamaño de la mano a repartir
        :return: Regresa la mejor mano posible, y su suma.
        '''
        # En caso de que el deck sea menor a la mano solicitada, regresamos la suma de todo el deck
        if hand_size >= len(deck):
            return deck, sum(deck)
    
        best_hand_sum = 0
        dealt_hands = []
    
        for num_top in range(hand_size + 1):
            num_bottom = hand_size - num_top
            # Repartimos cartas de abajo hasta 'num_bottom'
            dealt_hand = deck[0:num_bottom]
            hand_sum = sum(deck[0:num_bottom])
            
            # Repartimos 'num_top' cartas de arriba solo si 'num_top' es diferente de 0
            if num_top != 0:
                top_hand = deck[-num_top:]
                top_sum = sum(deck[-num_top:]) if num_top else 0
                hand_sum = hand_sum + top_sum
                dealt_hand = dealt_hand + top_hand
            
            # Actualizamos best_hand_sum y best_hand de ser necesario
            new_max = max(best_hand_sum, hand_sum)
            if new_max > best_hand_sum:
                best_hand_sum = new_max
                best_hand = dealt_hand
            
        return best_hand, best_hand_sum   

Este código tiene una complejidad de O(n2) ya que el número de sumas depende del tamaño de la mano "hand_size". Ejemplo:

| hand size | sum operations |
| --------- | -------------- |
| 2 | 4 |
| 3 | 9 |
| 4 | 16 |
| 5 | 25 |

## Prueba del código con diferentes manos

    import random

    start = time.time()
    mano, suma = get_best_hand_from_deck([3,1,1,6,2], 3)
    end = time.time()
    total_time = round(end-start,6)

    print(f'\nLa mejor mano fue {mano}, con valor de {suma}')
    print(f'Este algorítmo resolvió el problema en {total_time} segundos')
    print(f'\n')
        
    start = time.time()        
    mano, suma = get_best_hand_from_deck([5,4,2,7,7,6,4,1,1,1,9,7], 6)
    end = time.time()
    total_time = round(end-start,6)
    print(f'\nLa mejor mano fue {mano}, con valor de {suma}')
    print(f'Este algorítmo resolvió el problema en {total_time} segundos')
    print(f'\n')

    start = time.time()        
    mano, suma = get_best_hand_from_deck([5,4,2,7,7,6,4,1,1,1,9,7], 9)
    end = time.time()
    total_time = round(end-start,6)
    print(f'\nLa mejor mano fue {mano}, con valor de {suma}')
    print(f'Este algorítmo resolvió el problema en {total_time} segundos')
    print(f'\n')

    start = time.time()        
    mano, suma = get_best_hand_from_deck([5,4,2,7,7,6,4,1,1,1,9,7], 12)
    end = time.time()
    total_time = round(end-start,6)
    print(f'\nLa mejor mano fue {mano}, con valor de {suma}')
    print(f'Este algorítmo resolvió el problema en {total_time} segundos')
    print(f'\n')


Out: 

La mejor mano fue [3, 6, 2], con valor de 11
Este algorítmo resolvió el problema en 0.000101 segundos

La mejor mano fue [5, 4, 2, 7, 9, 7], con valor de 34
Este algorítmo resolvió el problema en 0.000112 segundos

La mejor mano fue [5, 4, 2, 7, 7, 6, 4, 9, 7], con valor de 51
Este algorítmo resolvió el problema en 0.000115 segundos

La mejor mano fue [5, 4, 2, 7, 7, 6, 4, 1, 1, 1, 9, 7], con valor de 54
Este algorítmo resolvió el problema en 0.000101 segundos


##  Utilizando el deck más largo permitido y entregando 8 mil cartas

    large_deck = [random.randint(1,9) for x in range(1000000)]
    start = time.time()        
    mano, suma = get_best_hand_from_deck(large_deck, 8000)
    end = time.time()
    total_time = round(end-start,6)
    print(f'\nLa mejor mano fue {mano}, con valor de {suma}')
    print(f'\nEste algorítmo resolvió el problema en {total_time} segundos')
    print(f'\n\n')

Out: 

La mejor mano fue [7, 3, 7, 9, 2, 1, 8, 5, 3, 9, 2, 3, 2, 3, 8, 8, 6, 1, 7, 4, 4, 4, 1, 1, 2, 8, 1, 5, 3, 7, 7, 2, 8, 3, 7, 3, 5, 7, 4, 2, 1, 3, 9, 4, 1, 7, 6, 1, 8, 9, 7, 2, 7, 5, 1, 1, 7, 1, 7, 3, 1, 5, 9, 3, 9, 4, 7, 6, 1, 4, 9, 1, 4, 3, 8, 9, 2, 2, 2, 9, 8, 4, 9, 7, 3, 7, 4, 8, 8, 8, 9, 9, 1, 5, 5, 4, 9, 7, 7, 3, 5, 9, 3, 4, 4, 1, 2, 5, 7, 1, 5, 1, 3, 5, 5, 2, 2, 3, 7, 4, ...
...
...
, 1, 6, 3, 2, 8, 7, 1, 4, 9, 1, 4, 8, 4, 3, 2, 1, 6, 2, 3, 9, 5, 5, 7, 3, 1, 8, 2, 9, 4, 2, 4, 4, 1, 3], con valor de 40170

Este algorítmo resolvió el problema en 1.015973 segundos

## Solución 2. Programación dinámica
tiempo O(n)

Podemos optimizar la solución previa evitando la sumatoria redundante sumamos los valores de las manos, reusando las sumas que ya han sido realizadas.

Por ejemplo: La siguiente mano:

abajo -->    3, 1, 1, 6, 2   <-- arriba
2 cartas     3, 1        2       1 carta

Para la siguiente mano, la iteración difiere con la anterior en solo 2 cartas. La 1 se remueve y la 6 se agrega.

abajo -->    3, 1, 1, 6, 2   <-- arriba
2 cartas     3, 1        2       1 carta
1 carta      3        6, 2       2 cartas

El valor de la nueva mano puede ser computado del valor de la mano previa en O(1) pasos sustrayendo 1 y agregando 6.

Basado en este razonamiento podemos reescribir el algoritmo, pero igual que el código anterior, este fue modificado para almacenar la mejor mano y entregar tanto la suma como la mejor mano. El problema a resolver es no realizar la asignación de esta mano cada que avanza el programa ya que estaríamos aumentando la complejidad a O(n2). Es por eso que solo almacenamos dos indicadores en el momento que encontramos la mejor mano y armamos el set con estos datos al final, conservando la complejidad en O(n)

    def get_best_hand_from_deck_2(deck: List, hand_size: int):
        '''
        :param deck: Lista de números que representan las cartas con valor númerico
        :param hand_size: número entero, el tamaño de la mano a repartir
        :return: Regresa la mejor mano posible, y su suma.
        '''
        
        # Handle the case where the deck is too small.
        if hand_size >= len(deck):
            return deck, sum(deck)
        
        # Empezamos entregando una mano con cartas solo de abajo del deck    num_top = 0
        num_bottom = hand_size
        hand = deck[0:num_bottom]
        hand_sum = sum(hand)

        # Llevamos el rastro de la mejor mano
        best_hand_sum = hand_sum
        
        num_top = 0
        while num_top < hand_size:
            
            # Como ya tenemos la suma de las tres cartas de abajo, solo
            # restamos el valor de las cartas de abajo que serán remplazadas
            # por las de arriba
            num_bottom -= 1
            num_top += 1
            hand_sum -= deck[num_bottom]
            hand_sum += deck[-num_top]
            
            # La siguiente linea no la uso, la muestro solo como una forma
            # en que se puede almacenar la mejor mano, con la desventaja de 
            # aumentar la complejidad del algorítmo. 
            
            # hand = deck[:num_bottom] + deck[-num_top:]
            
            # Actualizamos la mejor mano de ser necesario.
            new_hand_sum = max(best_hand_sum, hand_sum)
            if best_hand_sum < new_hand_sum:
                best_hand_sum = new_hand_sum
                best_hand = hand
                best_num_top = num_top
                best_num_bottom = num_bottom
        
        # Con las siguientes líneas armamos la lista con la mejor mano 
        # encontrada
        if num_top:
            best_hand = deck[0:num_bottom] + deck[-num_top:]
        else:
            best_hand = deck[0:num_bottom]
        
        return best_hand, best_hand_sum


## Prueba con un deck pequeño

    get_best_hand_from_deck_2([8,2,11,13,1], 2)

Out: ([13, 1], 14)


## Con el deck más largo permitido y entregando 8mil cartas

    start = time.time()        
    mano, suma = get_best_hand_from_deck_2(large_deck, 8000)
    end = time.time()
    total_time = round(end-start,6)
    print(f'\nLa mejor mano fue {mano}, con valor de {suma}')
    print(f'\nEste algorítmo resolvió el problema en {total_time} segundos')
    print(f'\n\n')

Out:

La mejor mano fue [9, 5, 6, 8, 7, 8, 1, 7, 5, 9, 9, 9, 5, 6, 6, 4, 7, 7, 4, 3, 7, 8, 1, 2, 9, 5, 3, 7, 8, 1, 3, 4, 9, 6, 1, 6, 1, 2, 8, 6, 7, 9, 5, 7, 7, 6, 3, 7, 2, 9, 9, 5, 9, 8, 8, 2, 4, 4, 9, 5, 4, 8, 3, 8, 9, 9, 8, 3, 8, 1, 6, 6, 9, 6, 9, 2, 4, 2, 5, 8, 7, 9, 1, 6, 4, 2, 7, 4, 9, 2, 2, 4, 7, 1, 5, 2, 5, 1, 2, 4, 6, 6, 9, 4, 7, 4, 8, 4, 1, 8, 1, 4, 2, 5, 4, 6, 9, 2, 7, 7, 7, 5, 9, 5, 6, 7, 2, 1, 2, 2, 6, 1, 5, 2, 4, 7, 4, 3, 6, 9, 3, 4, 3, 5, 4, 9, 3, 4, 7, 4, 6, 4, 5, 3, 2, 2, 3, 1, 2, 7, 2, 2, 8, 2, 3, 5, 2, 1, 6, 3, 7, 4, 1, 5, 8, 8, 8, 5, ...
...
...
...
9, 2, 6, 2, 2, 4, 4, 4, 8, 8, 8, 3, 2, 3, 7, 8, 4, 4, 8, 6, 9, 6, 7, 3, 4, 8, 3, 9, 5, 8, 8, 5, 5, 9, 3, 7, 8, 1, 6, 3, 2, 8, 7, 1, 4, 9, 1, 4, 8, 4, 3, 2, 1, 6, 2, 3, 9, 5, 5, 7, 3, 1, 8, 2, 9, 4, 2, 4, 4, 1, 3], con valor de 40170

Este algorítmo resolvió el problema en 0.004682 segundos

Podemos ver que la diferencia en tiempo de ejecución de los dos códigos es muy grande. El primero tardó .947 segundos, mientras que el segundo tardó 0.006 segundos. Es decir el primero tardó casi 150 veces lo que el segundo código.

## Códigos exactamente como en el libro

    def get_best_hand_from_deck_book_a(deck, hand_size):
        # Handle the case where the deck is too small. 
        if hand_size >= len(deck):
            return sum(deck)
        # Keep track of the best known hand. 
        best_hand = 0
        for num_top in range(hand_size + 1):
            num_bottom = hand_size - num_top
            # Deal num_bottom cards from the bottom...
            bottom = sum(deck[0:num_bottom])
            # and num_top cards from the top.
            top = sum(deck[-num_top:]) if num_top else 0 
            hand = bottom + top
            # Update the best hand if needed.
            best_hand = max(best_hand, hand)
        return best_hand


    def get_best_hand_from_deck_book_b(deck, hand_size):
        # Handle the case where the deck is too small. 
        if hand_size >= len(deck):
            return sum(deck)
        # Start with a hand dealing only from the bottom of the deck
        num_top = 0
        num_bottom = hand_size
        hand = sum(deck[0:num_bottom])
        # keep track of the best known hand.
        best_hand = hand
        while num_top < hand_size:
            # Give up the deepest card from the bottom,
            # replacing it with a card dealt from the top.
            num_bottom -= 1
            num_top += 1
            hand -= deck[num_bottom]
            hand += deck[-num_top]
            #Update the best hand if needed.
            best_hand = max(best_hand, hand)
        return best_hand


    start = time.time()        
    suma = get_best_hand_from_deck_book_b(large_deck, 8000)
    end = time.time()
    total_time = round(end-start,6)
    print(f'\nLa mejor mano fue con valor de {suma}')
    print(f'Este algorítmo resolvió el problema en {total_time} segundos')
    print(f'\n\n')

Out: 

La mejor mano fue con valor de 40170
Este algorítmo resolvió el problema en 0.004916 segundos

## Comparativo gráfico de las funciones

    import numpy as np
    import matplotlib.pyplot as plt

    def total_time(deck: List, hand_size: int):
        start = time.time()
        get_best_hand_from_deck(deck, hand_size)
        end = time.time()
        f1time = end - start
        
        return f1time

    def total_time_2(deck: List, hand_size: int):
        start2 = time.time()
        get_best_hand_from_deck_2(deck, hand_size)
        end2 = time.time()
        f2time = end2 - start2
        
        return f2time

    def total_time_3(deck: List, hand_size: int):
        start2 = time.time()
        get_best_hand_from_deck_book_b(deck, hand_size)
        end2 = time.time()
        f2time = end2 - start2
        
        return f2time

    total_time(large_deck,4)

    def compareAsymptotic(deck: List, hand_size: int, step: int):
        x = np.arange(2, hand_size, step)
        y = [total_time(deck, xe) for xe in x]
        y2 = [total_time_2(deck, xe) for xe in x]
        y3 = [total_time_3(deck, xe) for xe in x]
        plt.title(f'O(n^2) vs O(n)')
        plt.plot(x, y, 'r', label='Fuerza bruta')
        plt.plot(x, y2, 'b', label='Mi solución dinámica')
        plt.plot(x, y3, 'y', label='Solución dinámica (Libro)')
        plt.legend()
        plt.show()
        
    def compareAsymptotic2(deck: List, hand_size: int, step: int):
        x = np.arange(3, hand_size, step)
        y2 = [total_time_2(deck, xe) for xe in x]
        y3 = [total_time_3(deck, xe) for xe in x]
        plt.title(f'O(n)')
        plt.plot(x, y2, 'b', label='Mi solución dinámica')
        plt.plot(x, y3, 'g', label='Solución dinámica (Libro)')
        plt.legend()
        plt.show()    
        
    compareAsymptotic(large_deck, 500, 20)

    compareAsymptotic2(large_deck, 100000, 1000)  

Out:

<div class="output_png output_subarea ">
<img src="data:image/png;base64,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Xr144IEHePTRR3n66ZOLh7Vs2ZLdu3czceJEfv31V9q0acP9999PWloaffr0oV27dsTFxRXkWSlViXbsgDFjoGtXeOyxKj117aox3HMPVOAvdwDatIHnny/zkIyMDNq0aQNAkyZNmD9/fqnHPv744/Tu3ZtZs2aRkpJCx44d6du3LwA//vgjmzZtIjw8HIApU6Zw9OhRLrvsMsaNGwfArFmzCA8PJyMjgw4dOjB06FAiIiJOOU+dOnVYvXo1b731Fvfccw+LFi0CYMeOHSxZsgQfHx8effTREvM4depUNm/eXFALysnJYf78+dSpU4fDhw/TqVMnBg8erB2uSlWWzEz461/Bz8/WGiq5BaG42hUYPKQ8TUlff/01CxcuLPilnpmZyd69ewH4y1/+UhAUwI6PHz58OBMmTKB9+/YATJ8+vSDw7Nu3j507d5YYGIYNG1bwOGHChIL91113HT4+PuUqnzGGBx98kOXLl+NwOPjjjz84ePAg55xzTrk+RynlpnvvhZ9+gkWLoFGjKj997QoMp/llX5WcTid5eXkARca3G2P46KOPaNasWZHjV61aRXBwcJF9jz76KDExMQXNSMuWLWPJkiX8+OOPBAUF0bNnz1LHzhf+NV/4eeFzFM5j8XwW9u6775KYmMi6devw9fUlNjZWx+wrVVnefx9mzID77oOBAz2SBe1jqCSxsbGsW7cOgI8++qhg/+WXX86LL75I/lrbG0oZabBo0SIWL17M9OnTC/YdPXqUevXqERQUxLZt21i5cmWp5583b17BY+fOnUvN4/r16wFYv349v//+OwChoaGkpqYWOW90dDS+vr4sXbqUPXvKNYOvUspdv/4Kt90GnTrBE094LBu1q8ZQjTzyyCPceuutPPHEE1x66aUF+x9++GHuueceWrVqhTGG2NjYgvb/wp555hn2799Px44dARg8eDAPPfQQr776Kq1ataJZs2Z06tSp1PNnZWVx6aWXkpeXx5w5c0o8ZujQobz11lu0adOGDh06cNFFFwEQERFB165dadmyJQMGDOCBBx5g0KBBxMfH06ZNG5o3b342X41SqiRZWbZfwemEuXPBg/NuSf4v15ogPj7eFF+oZ+vWrVx88cUeypFSJ+nfojor48fDiy/CggUweHCFfrSIrDPGxLt7vDYlKaWUp330kQ0KEyZUeFA4ExoYlFLKk377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
"
>
</div>


<div class="output_png output_subarea ">
<img src="data:image/png;base64,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
"
>
</div>


## Conclusión

En la primera gráfica podemos ver como el primer código "Fuerza Bruta" crece exponencialmente por lo que con una gran cantidad de datos deja de ser útil. Para pequeños programas que no trabajarán con muchos datos, esta forma de programar no hará gran diferencia, o al menos no será notoria humanamente.

En la segunda gráfica podemos ver que la solución dinámica donde agregé código para entregar además de la suma, la mano que daba ese resultado, crece linearmente casi a la par que la solución dinámica del libro. Es decir, sigue manteniendo su eficiencia aún con una gran cantidad de datos.

Un buen código puede hacer una gran diferencia.

