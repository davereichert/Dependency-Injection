# Dependency Injection (DI)

## Sinn und Zweck

Dependency Injection ist ein Software-Design-Muster, das darauf abzielt, die Abhängigkeiten eines Objekts zu entkoppeln. Durch die Entkopplung von Abhängigkeiten:

- Erhöht sich die Testbarkeit, da Sie leicht Mock- oder Stub-Implementierungen bereitstellen können.
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
