
class Fraction:
    """Classe représentant une fraction et ses opérations."""

    def __init__(self, num=0, den=1):
        """Construit une fraction avec un numérateur et un dénominateur.
        
        PRE : Le dénominateur ne doit pas être égal à zéro.
        POST : La fraction est créée avec les valeurs données.
        """
        if den == 0:
            raise ValueError("Le dénominateur doit être autre que 0")
        self.num = num
        self.den = den

        self.simplify()

    def simplify(self):
        """Simplifie la fraction à sa forme la plus simple.
        
        PRE : La fraction est initialisée.
        POST : La fraction est simplifiée.
        """
        a, b = abs(self.num), abs(self.den)
        while b != 0:
            a, b = b, a % b
        gcd = a

        self.num //= gcd
        self.den //= gcd

        # Si le dénominateur est négatif, déplacer le signe négatif au numérateur
        if self.den < 0:
            self.num = -self.num
            self.den = -self.den

    @property
    def numerator(self):
        """Retourne le numérateur de la fraction."""
        return self.num

    @property
    def denominator(self):
        """Retourne le dénominateur de la fraction."""
        return self.den

    def __str__(self):
        """Retourne la représentation textuelle de la fraction.
        
        PRE : La fraction est initialisée.
        POST : La fraction est représentée sous forme de chaîne de caractères 'numérateur/dénominateur'.
        """
        return f'{self.num}/{self.den}'

    def as_mixed_number(self):
        """Returns the fraction as a mixed number.
    
        PRE : Fraction is initialized.
        POST : Returns the mixed number representation or integer.
        """
        if self.num == 0:
            return "0"  # Fraction is zero
    
        # Gestion du signe : si le numérateur est négatif, la fraction entière aussi sera négative
        is_negative = self.num < 0
    
        abs_num = abs(self.num)
        abs_den = abs(self.den)
    
        if abs_num < abs_den:
            return f"{'-' if is_negative else ''}{abs_num}/{abs_den}"  # Fraction propre (ex : 3/4)
    
        integer_part = abs_num // abs_den  # Partie entière
        remainder = abs_num % abs_den  # Reste de la division
    
        if remainder == 0:
            return f"{'-' if is_negative else ''}{integer_part}"  # C'est un entier
        else:
            return f"{'-' if is_negative else ''}{integer_part} {remainder}/{abs_den}"  # Nombre mixte


    # ------------------ Surcharge des opérateurs ------------------

    def __add__(self, other):
        """Surcharge de l'opérateur + pour les fractions.      
        PRE : Les deux objets (self et other) doivent être des instances de la classe Fraction.
             Si l'un des objets n'est pas une instance de Fraction, une exception TypeError est levée.
        POST : Retourne une nouvelle fraction représentant la somme des deux fractions, simplifiée.
        """
        if not isinstance(other, Fraction):  # Vérifie que 'other' est bien une fraction
            raise TypeError("L'opération d'addition nécessite une autre fraction.")
        num = self.num * other.den + other.num * self.den
        den = self.den * other.den
        return Fraction(num, den)


    def __sub__(self, other):
        """Surcharge de l'opérateur - pour les fractions.
        
        PRE : Les deux fractions sont initialisées.
        POST : Retourne la différence des deux fractions sous forme de fraction simplifiée.
        """
        num = self.num * other.den - other.num * self.den
        den = self.den * other.den
        return Fraction(num, den)

    def __mul__(self, other):
        """Surcharge de l'opérateur * pour les fractions.
        
        PRE : Les deux fractions sont initialisées.
        POST : Retourne le produit des deux fractions sous forme de fraction simplifiée.
        """
        num = self.num * other.num
        den = self.den * other.den
        return Fraction(num, den)

    def __truediv__(self, other):
        """Surcharge de l'opérateur / pour les fractions.
        
        PRE : Le dénominateur de la fraction de l'autre n'est pas nul.
        POST : Retourne le quotient des deux fractions sous forme de fraction simplifiée.
        """
        if other.num == 0:
            raise ZeroDivisionError("La division par une fraction nulle n'est pas définie.")
        num = self.num * other.den
        den = self.den * other.num
        return Fraction(num, den)

    def __pow__(self, other):
        """Surcharge de l'opérateur ** pour les puissances de fractions.
        
        PRE : La fraction est initialisée, et l'autre argument est soit un entier, soit une fraction.
        POST : Retourne la fraction élevée à la puissance de l'autre fraction sous forme de fraction simplifiée.
        """
        if isinstance(other, int):  # Si l'exposant est un entier
            num = self.num ** other
            den = self.den ** other
        elif isinstance(other, Fraction):  # Si l'exposant est une fraction
            num = self.num ** other.num
            den = self.den ** other.num
        else:
            raise TypeError("L'exposant doit être un entier ou une fraction.")

        return Fraction(num, den)

    def __eq__(self, other):
        """Surcharge de l'opérateur == pour comparer les fractions.
        
        PRE : Les deux fractions sont initialisées.
        POST : Retourne True si les fractions sont égales, sinon False.
        """
        return self.num == other.num and self.den == other.den

    def __float__(self):
        """Retourne la valeur décimale de la fraction.
        
        PRE : La fraction est initialisée.
        POST : Retourne la valeur décimale de la fraction.
        """
        return self.num / self.den

    # ------------------ Vérifications des propriétés ------------------

    def is_zero(self):
        """Vérifie si la fraction est nulle.
        
        PRE : La fraction est initialisée.
        POST : Retourne True si la fraction est nulle, sinon False.
        """
        return self.num == 0

    def is_integer(self):
        """Vérifie si la fraction est un entier (c'est-à-dire, numérateur divisible par dénominateur).
        
        PRE : La fraction est initialisée.
        POST : Retourne True si la fraction est un entier, sinon False.
        """
        return self.num % self.den == 0

    def is_proper(self):
        """Vérifie si la fraction est propre (valeur absolue < 1).
        
        PRE : La fraction est initialisée.
        POST : Retourne True si la fraction est propre, sinon False.
        """
        return abs(self.num) < abs(self.den)

    def is_unit(self):
        """Vérifie si la fraction est une fraction unitaire (numérateur égal à 1).
        
        PRE : La fraction est initialisée.
        POST : Retourne True si la fraction est unitaire, sinon False.
        """
        return self.num == 1

    def is_adjacent_to(self, other):
        """Vérifie si deux fractions diffèrent d'une fraction unitaire.
        
        PRE : Les deux fractions sont initialisées.
        POST : Retourne True si les fractions diffèrent d'une fraction unitaire, sinon False.
        """
        diff = abs(self.num * other.den - other.num * self.den)
        return diff == 1
if __name__ == "__main__":
    # Cas de tests avec différentes fractions
    # Fraction = 0/5
    f1 = Fraction(0, 5)
    print(f1.as_mixed_number())  # Devrait afficher "0" (fraction nulle)
    
    # Fraction propre = 3/4
    f2 = Fraction(3, 4)
    print(f2.as_mixed_number())  # Devrait afficher "3/4" (pas de partie entière)
    
    # Fraction impropre = 7/3
    f3 = Fraction(7, 3)
    print(f3.as_mixed_number())  # Devrait afficher "2 1/3" (partie entière et fraction)

    # Fraction entière = 5/1
    f4 = Fraction(5, 1)
    print(f4.as_mixed_number())  # Devrait afficher "5" (partie entière seule)

    # Fraction entière simplifiée = 10/5
    f5 = Fraction(10, 5)
    print(f5.as_mixed_number())  # Devrait afficher "2" (partie entière)

    # Fraction négative propre = -3/4
    f6 = Fraction(-3, 4)
    print(f6.as_mixed_number())  # Devrait afficher "-3/4" (pas de partie entière)

    # Fraction négative impropre = -7/3
    f7 = Fraction(-7, 3)
    print(f7.as_mixed_number())  # Devrait afficher "-2 1/3" (partie entière et fraction)

    # Fraction avec numérateur et dénominateur négatifs = -3/-4
    f8 = Fraction(-3, -4)
    print(f8.as_mixed_number())  # Devrait afficher "3/4" (les signes s'annulent)

    # Fraction = 10/10 (fraction égale à 1)
    f9 = Fraction(10, 10)
    print(f9.as_mixed_number())  # Devrait afficher "1" (fraction simplifiée en entier)

    # Addition de fractions = 7/3 + 3/4
    f10 = Fraction(7, 3)
    f11 = Fraction(3, 4)
    print((f10 + f11))  # Devrait afficher la somme en forme de fraction mixte

    # Soustraction de fractions = 7/3 - 3/4
    print((f10 - f11))  # Devrait afficher la différence en forme de fraction mixte

    # Multiplication de fractions = 7/3 * 3/4
    print((f10 * f11))  # Devrait afficher le produit en forme de fraction mixte

    # Division de fractions = 7/3 / 3/4
    print((f10 / f11))  # Devrait afficher le quotient en forme de fraction mixte

    # Exponentiation de fraction = (7/3) ** 2
    print((f10 ** 2))  # Devrait afficher le carré de la fraction

    # Comparaison de fractions = 7/3 == 14/6
    f12 = Fraction(14, 6)
    print(f10 == f12)  # Devrait afficher True car 7/3 == 14/6 (simplifié)

    # Test de conversion en float = 7/3
    print(float(f10))  # Devrait afficher le résultat décimal de 7/3