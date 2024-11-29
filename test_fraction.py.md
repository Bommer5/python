import unittest
from fraction import Fraction  # Assurez-vous que le fichier s'appelle fraction.py

class TestFraction(unittest.TestCase):

    def test_init(self):
        # Test de l'initialisation d'une fraction
        f1 = Fraction(3, 4)
        self.assertEqual(f1.numerator, 3)
        self.assertEqual(f1.denominator, 4)
        
        # Test de la simplification automatique
        f2 = Fraction(6, 8)  # La fraction doit être simplifiée à 3/4
        self.assertEqual(f2.numerator, 3)
        self.assertEqual(f2.denominator, 4)

        # Test de l'exception lorsque le dénominateur est 0
        with self.assertRaises(ValueError):
            Fraction(1, 0)

    def test_simplify(self):
        # Test de la méthode de simplification
        f1 = Fraction(10, 20)
        self.assertEqual(f1.numerator, 1)
        self.assertEqual(f1.denominator, 2)

        f2 = Fraction(-6, 9)
        self.assertEqual(f2.numerator, -2)
        self.assertEqual(f2.denominator, 3)

    def test_str(self):
        # Test de la représentation en chaîne
        f1 = Fraction(3, 4)
        self.assertEqual(str(f1), "3/4")
        
        f2 = Fraction(-5, 10)
        self.assertEqual(str(f2), "-1/2")

    def test_as_mixed_number(self):
        # Test de la conversion en nombre mixte
        f1 = Fraction(7, 3)
        self.assertEqual(f1.as_mixed_number(), "2 1/3")
        
        f2 = Fraction(4, 3)
        self.assertEqual(f2.as_mixed_number(), "1 1/3")
        
        f3 = Fraction(4, 2)
        self.assertEqual(f3.as_mixed_number(), "2")

    def test_add(self):
        # Test de l'addition de fractions
        f1 = Fraction(1, 2)
        f2 = Fraction(1, 3)
        result = f1 + f2
        self.assertEqual(result.numerator, 5)
        self.assertEqual(result.denominator, 6)

        # Addition avec une fraction négative
        f3 = Fraction(-1, 2)
        result = f1 + f3
        self.assertEqual(result.numerator, 0)
        self.assertEqual(result.denominator, 1)

    def test_sub(self):
        # Test de la soustraction de fractions
        f1 = Fraction(5, 6)
        f2 = Fraction(1, 3)
        result = f1 - f2
        self.assertEqual(result.numerator, 1)
        self.assertEqual(result.denominator, 2)

    def test_mul(self):
        # Test de la multiplication de fractions
        f1 = Fraction(2, 3)
        f2 = Fraction(3, 4)
        result = f1 * f2
        self.assertEqual(result.numerator, 1)
        self.assertEqual(result.denominator, 6)
        
        # Vérification de la simplification
        f3 = Fraction(4, 8)
        self.assertEqual(f3.numerator, 1)
        self.assertEqual(f3.denominator, 2)

    def test_div(self):
        # Test de la division de fractions
        f1 = Fraction(2, 3)
        f2 = Fraction(3, 4)
        result = f1 / f2
        self.assertEqual(result.numerator, 8)
        self.assertEqual(result.denominator, 9)

        # Test de la division par une fraction nulle
        with self.assertRaises(ZeroDivisionError):
            f1 / Fraction(0, 1)

    def test_eq(self):
        # Test de la comparaison de fractions
        f1 = Fraction(1, 2)
        f2 = Fraction(2, 4)
        self.assertTrue(f1 == f2)

        f3 = Fraction(3, 4)
        self.assertFalse(f1 == f3)

    def test_float(self):
        # Test de la conversion en flottant
        f1 = Fraction(1, 2)
        self.assertEqual(float(f1), 0.5)

        f2 = Fraction(3, 4)
        self.assertEqual(float(f2), 0.75)

    def test_is_zero(self):
        # Test pour vérifier si la fraction est nulle
        f1 = Fraction(0, 5)
        self.assertTrue(f1.is_zero())

        f2 = Fraction(1, 5)
        self.assertFalse(f2.is_zero())

    def test_is_integer(self):
        # Test pour vérifier si la fraction est un entier
        f1 = Fraction(4, 2)
        self.assertTrue(f1.is_integer())

        f2 = Fraction(3, 4)
        self.assertFalse(f2.is_integer())

    def test_is_proper(self):
        # Test pour vérifier si la fraction est propre (num < den)
        f1 = Fraction(3, 4)
        self.assertTrue(f1.is_proper())

        f2 = Fraction(5, 4)
        self.assertFalse(f2.is_proper())

    def test_is_unit(self):
        # Test pour vérifier si la fraction est unitaire (num = 1)
        f1 = Fraction(1, 5)
        self.assertTrue(f1.is_unit())

        f2 = Fraction(2, 5)
        self.assertFalse(f2.is_unit())

    def test_is_adjacent_to(self):
        # Test pour vérifier si deux fractions sont adjacentes
        f1 = Fraction(2, 3)
        f2 = Fraction(3, 4)
        self.assertTrue(f1.is_adjacent_to(f2))

        f3 = Fraction(1, 2)
        self.assertFalse(f1.is_adjacent_to(f3))


if __name__ == '__main__':
    unittest.main()
