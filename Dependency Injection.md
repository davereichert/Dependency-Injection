# Dependency Injection (DI)

## Sinn und Zweck

Dependency Injection ist ein Software-Design-Muster, das darauf abzielt, die Abhängigkeiten eines Objekts zu entkoppeln. Durch die Entkopplung von Abhängigkeiten:

- Erhöht sich die Testbarkeit
- Wird der Code modularer und flexibler, da Komponenten einfacher ausgetauscht werden können.
- Verbessert sich die Wartbarkeit, da Änderungen in einer Komponente weniger Auswirkungen auf andere haben.

## Code Beispielprojekt

### Mit Dependency Injection:

```csharp
public class Gluehbirne 
{
    public virtual string Leuchten() 
    {
        return "Helle Glühbirne";
    }
}

public class LEDBirne : Gluehbirne
{
    public override string Leuchten()
    {
        return "LED Licht";
    }
}

public class Lampe 
{
    private readonly Gluehbirne _birne;

    public Lampe(Gluehbirne birne) 
    {
        this._birne = birne;
    }

    public string Einschalten() 
    {
        return this._birne.Leuchten();
    }
}
```
### Ohne Dependency Injection:

```csharp
public class Lampe 
{
    private readonly Gluehbirne _birne = new Gluehbirne();

    public string Einschalten() 
    {
        return this._birne.Leuchten();
    }
}
```
### Main Methode mit dem Dependency Injection:

```csharp
public class Program
{
    public static void Main()
    {
        Lampe normaleLampe = new Lampe(new Gluehbirne());
        Console.WriteLine(normaleLampe.Einschalten());  // Ausgabe: Helle Glühbirne

        Lampe ledLampe = new Lampe(new LEDBirne());
        Console.WriteLine(ledLampe.Einschalten());  // Ausgabe: LED Licht
    }
}
```

# Grundkonzept von DIP

- **High-Level-Module** sollten nicht von **Low-Level-Modulen** abhängig sein. Beide sollten von Abstraktionen abhängig sein.
- **Abstraktionen** sollten nicht von Details abhängig sein. **Details** sollten von Abstraktionen abhängig sein.

## Was macht eine Klasse zu einer High-Level-Klasse?

High-Level-Klassen steuern den Ablauf eines Programms und definieren Systemverhalten. Sie repräsentieren die Systempolitik. In unserem Beispiel ist die `Switch` Klasse eine High-Level-Klasse, da sie das Verhalten (das Bedienen eines Schalters) steuert.

### Beispiel: Falscher Ansatz

```csharp
public class LightBulb
{
    public void TurnOn()
    {
        // Logik zum Einschalten
    }
    public void TurnOff()
    {
        // Logik zum Ausschalten
    }
}

public class Switch
{
    private LightBulb _bulb;

    public Switch(LightBulb bulb)
    {
        _bulb = bulb;
    }

    public void Operate()
    {
        // Logik zum Bedienen des Schalters
    }
}
```

### Beispiel: Richtiger Ansatz

```csharp
public interface ISwitchable
{
    void TurnOn();
    void TurnOff();
}

public class LightBulb : ISwitchable
{
    public void TurnOn()
    {
        // Logik zum Einschalten
    }
    public void TurnOff()
    {
        // Logik zum Ausschalten
    }
}

public class Switch
{
    private ISwitchable _device;

    public Switch(ISwitchable device)
    {
        _device = device;
    }

    public void Operate()
    {
        // Logik zum Bedienen des Schalters
    }
}
```

# Factory Design Pattern

## Sinn und Zweck:

- **Einfachere Nutzung**: Du kannst Objekte leichter erstellen, ohne die komplizierten Details zu kennen.
- **Flexibilität**: Du kannst neue Objekte hinzufügen, ohne den alten Code zu ändern.
- **Kontrolle**: Du hast die Kontrolle darüber, wie Objekte erstellt werden.

Das Factory Design Pattern ist wie eine praktische Werkzeugkiste für das Erstellen von Objekten in deinem Code.

## Beispiel:

Wir haben ein einfaches Szenario, bei dem Tiere unterschiedliche Laute erzeugen können. Anstatt jedes Mal, wenn wir ein Tier erstellen wollen, den spezifischen Konstruktor aufzurufen, verwenden wir die `AnimalFactory` um uns das gewünschte Tier zu geben.

```csharp
public abstract class Animal
{
    public abstract string Speak();
}

public class Dog : Animal
{
    public override string Speak()
    {
        return "Woof!";
    }
}

public class Cat : Animal
{
    public override string Speak()
    {
        return "Meow!";
    }
}

public class AnimalFactory
{
    public Animal GetAnimal(string animalType)
    {
        switch (animalType)
        {
            case "Dog":
                return new Dog();
            case "Cat":
                return new Cat();
            default:
                return null;
        }
    }
}
```
## Verwendung:

Wenn der Client-Code ein Tier benötigt, verwendet er die Factory:

```csharp
AnimalFactory factory = new AnimalFactory();
Animal animal = factory.GetAnimal("Dog");
Console.WriteLine(animal.Speak());  // Ausgabe: "Woof!"

```

