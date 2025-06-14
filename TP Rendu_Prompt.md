

![image-20250614172900133](C:\Users\WALID\AppData\Roaming\Typora\typora-user-images\image-20250614172900133.png)



# TP : Ingénierie de prompt appliquée à la génération de code avec l’IA

lebhir walid GI1 . 

## Exercices 2.1

'''Prompt1 : "Écris une fonction pour faire des opérations entre deux nombres en Python."'''

def calculer(a, b, operation):
    '''
    Effectue une opération entre deux nombres.

    :param a: premier nombre
    :param b: deuxième nombre
    :param operation: une chaîne parmi '+', '-', '*', '/'
    :return: le résultat de l'opération
    '''
    if operation == '+':
        return a + b
    elif operation == '-':
        return a - b
    elif operation == '*':
        return a * b
    elif operation == '/':
        if b != 0:
            return a / b
        else:
            return "Erreur : division par zéro"
    else:
        return "Opération non reconnue"

'''Prompt 2 : plus spécifique avec gestion des erreurs et arrondi'''

def calculate(a, b, op):
    '''
    Effectue une opération mathématique entre deux entiers.

    Paramètres :
    a (int) : le premier entier
    b (int) : le deuxième entier
    op (str) : '+', '-', '*', '/'
    
    Retourne :
    float ou int : résultat
    str : message d'erreur
    '''
    if op == '+':
        return a + b
    elif op == '-':
        return a - b
    elif op == '*':
        return a * b
    elif op == '/':
        if b == 0:
            return "Erreur : division par zéro"
        else:
            return round(a / b, 2)
    else:
        return "Erreur : opération invalide"

'''Prompt 3 : avec Persona + PEP8 + docstring clair'''

def calculate(a, b, op):
    '''
    Effectue une opération mathématique de base entre deux entiers.

    Paramètres
    ----------
    a : int
        Le premier entier.
    b : int
        Le second entier.
    op : str
        '+', '-', '*', '/'.
    
    Retours
    -------
    float ou int : Résultat
    str : Message d'erreur
    '''
    if not isinstance(a, int) or not isinstance(b, int):
        return "Erreur : les deux paramètres doivent être des entiers"
    if op == '+':
        return a + b
    elif op == '-':
        return a - b
    elif op == '*':
        return a * b
    elif op == '/':
        if b == 0:
            return "Erreur : division par zéro"
        return round(a / b, 2)
    else:
        return "Erreur : opérateur invalide"

## Exercices 2.2

'''Prompt 1 : Zéro-shot'''

def format_product_code(product_id):
    '''
    Formate un identifiant produit de 10 caractères alphanumériques
    en insérant des tirets après les 3e et 7e caractères.
    '''
    if len(product_id) != 10 or not product_id.isalnum():
        raise ValueError("L'identifiant doit contenir exactement 10 caractères alphanumériques.")
    return f"{product_id[:3]}-{product_id[3:7]}-{product_id[7:]}"

'''Prompt 2 : One-shot avec exemple'''

def format_product_code(product_id):
    '''
    Formate un identifiant produit. Exemple :
    "ABC123DEF4" devient "ABC-123-DEF4"
    '''
    if len(product_id) != 10 or not product_id.isalnum():
        raise ValueError("L'identifiant doit contenir exactement 10 caractères alphanumériques.")
    return f"{product_id[:3]}-{product_id[3:6]}-{product_id[6:]}"

'''Prompt 3 : Multi-shot avec cas d'erreur'''

def format_product_code(product_id):
    '''
    Formate un identifiant produit. Exemples :
    - "ABC123DEF4" → "ABC-123-DEF4"
    - "XYZ987GHIJ" → "XYZ-987-GHIJ"
    - "SHORT" → ValueError
    '''
    if len(product_id) != 10 or not product_id.isalnum():
        raise ValueError("L'identifiant doit contenir exactement 10 caractères alphanumériques.")
    return f"{product_id[:3]}-{product_id[3:6]}-{product_id[6:]}"

## Partie 3 : Débogage

'''Fonction corrigée'''

def calculate_average(numbers_list):
    total = 0
    count = 0
    for num in numbers_list:
        if isinstance(num, (int, float)):
            total += num
            count += 1
    if count == 0:
        raise ValueError("Aucun nombre valide dans la liste.")
    return total / count

'''Exemple d'utilisation'''
my_nums = [1, 2, 'three', 4]
try:
    avg = calculate_average(my_nums)
    print(f"Moyenne : {avg}")
except ValueError as e:
    print(f"Erreur : {e}")

'''Tests unitaires avec pytest'''
import pytest
from typing import List

def calculate_average(numbers_list: List[float]) -> float:
    total = 0
    count = 0
    for num in numbers_list:
        if isinstance(num, (int, float)):
            total += num
            count += 1
    if count == 0:
        return 0
    return total / count

def test_average_all_integers():
    assert calculate_average([2, 4, 6]) == 4.0

def test_average_with_floats():
    assert calculate_average([1.5, 2.5, 3.0]) == pytest.approx(2.333, 0.001)

def test_average_with_non_numeric():
    assert calculate_average([1, 'two', 3]) == 2.0

def test_average_empty_list():
    assert calculate_average([]) == 0

def test_average_only_strings():
    assert calculate_average(['a', 'b', 'c']) == 0

def test_average_with_none():
    assert calculate_average([1, None, 2]) == 1.5

def test_average_mixed_types():
    assert calculate_average([1, '2', 3.5, True, [4], 0]) == pytest.approx((1 + 3.5 + 1 + 0)/4, 0.001)

## Partie 3.2 : Refactoring

def get_numbers():
    '''
    Retourne une liste d'entiers.
    '''
    return [5, 3, 8, 6, 7, 2]

def sort_numbers_ascending(numbers):
    '''
    Trie une liste d'entiers par ordre croissant (bubble sort).
    '''
    sorted_list = numbers.copy()
    length = len(sorted_list)
    for i in range(length):
        for j in range(i + 1, length):
            if sorted_list[i] > sorted_list[j]:
                '''Swap values'''
                sorted_list[i], sorted_list[j] = sorted_list[j], sorted_list[i]
    return sorted_list

def display_sorted_numbers(sorted_numbers):
    '''
    Affiche la liste triée.
    '''
    print("Sorted numbers in ascending order:", sorted_numbers)

if __name__ == "__main__":
    '''
    Point d'entrée principal du programme.
    '''
    numbers = get_numbers()
    sorted_numbers = sort_numbers_ascending(numbers)
    display_sorted_numbers(sorted_numbers)

## Partie 3.3 : Docstring et README

def get_user_permissions(user_id, system_context):
    '''
    Retrieve the list of permissions assigned to a user based on their role in the system.

    Args:
        user_id (str or int): The unique identifier of the user.
        system_context (dict): Contains user roles and associated user IDs.
    
    Returns:
        list of str: Permissions like 'read', 'write', 'delete', 'admin'.
    
    Example:
        >>> context = {
        ...     'admins': ['user1', 'user2'],
        ...     'editors': ['user3', 'user4']
        ... }
        >>> get_user_permissions('user1', context)
        ['read', 'write', 'delete', 'admin']
    '''
    if user_id in system_context.get('admins', []):
        return ['read', 'write', 'delete', 'admin']
    elif user_id in system_context.get('editors', []):
        return ['read', 'write']
    else:
        return ['read']