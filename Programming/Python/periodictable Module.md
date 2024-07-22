```powershell
pip install periodictable
```

```python
>>> from periodictable import *
>>> print(elements[26])
Fe
>>> print(elements.Fe)
Fe
>>> print(elements.symbol('Fe'))
Fe
>>> print(elements.name('iron'))
Fe
>>> print(elements.isotope('Fe'))
Fe
```

```python
import periodictable
t = 0
element = periodictable.elements.symbol('H')  # Retrieve information for hydrogen

# Accessing element properties
print(f"Element: {element.name}")
print(f"Symbol: {element.symbol}")
print(f"Atomic number: {element.number}")
print(f"Atomic weight: {element.mass}")
print(f"Electron Charge: {element.charge}")

# Listing all elements
for element in periodictable.elements:
    print(element.symbol, end=" ")
    t += 1
print(f"\nTotal elements in the periodic table: {t}")


# Accessing isotopes of an element
hydrogen_isotopes = periodictable.elements.symbol('H').isotopes
print(f"\nHydrogen isotopes: {hydrogen_isotopes}\n")

# Calculating molar mass of a compound
water = periodictable.formula("H2O")
molar_mass = water.mass
print(f"Molar mass of water (H2O): {molar_mass} g/mol")

# Accessing constants
avogadro_number = periodictable.constants.avogadro_number
print(f"Avogadro's number: {avogadro_number}")
```
