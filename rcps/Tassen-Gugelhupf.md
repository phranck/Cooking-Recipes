# Tassen Gugelhupf

Nun, ich verliere gar nicht viele Worte, sondern komme gleich zum Punkt. Dem Code, zu diesem leckeren Kuchen. 🤓

Das Ganze gibt es [hier als Swift-Playground](./Tassen-Gugelhupf.zip). Herunterladen, in Xcode oeffnen und starten.  \
Bon apetit!

```swift
import Foundation

internal extension Double {
    var naturalValue: String {
        let integerPart = Int(self)
        let fractionalPart = Double(integerPart) - self

        return fractionalPart == 0 ? "\(integerPart)" : "\(self)"
    }
}

public enum Unit {
    case tasse(Double)
    case paeckchen(Double)
    case prise(Int)
    case essloeffel(Int)
    case stueck(Int)
    case milliliter(Int)
    case gramm(Double)

    var name: String {
        switch self {
        case .tasse(let amount):
            return "Tasse\(amount > 1 || amount < 1 ? "n" : "")"
        case .paeckchen:
            return "Pck"
        case .prise:
            return "Prise"
        case .essloeffel:
            return "EL"
        case .stueck:
            return "Stck"
        case .milliliter:
            return "ml"
        case .gramm:
            return "g"
        }
    }

    var quantity: String {
        switch self {
        case .tasse(let quantity):
            return "\(quantity.naturalValue) \(name)"
        case .paeckchen(let quantity):
            return "\(quantity.naturalValue) \(name)"
        case .prise(let quantity):
            return "\(quantity) \(name)"
        case .essloeffel(let quantity):
            return "\(quantity) \(name)"
        case .stueck(let quantity):
            return "\(quantity) \(name)"
        case .milliliter(let quantity):
            return "\(quantity) \(name)"
        case .gramm(let quantity):
            return "\(quantity) \(name)"
        }
    }
}

internal protocol Preparation {
    func listIngredients() async
    func prepareIngredients() async
    func prepareOven(temperature: Int, circulatingAir: Bool)
    func fillOven(with circulatingAir: Bool)
}

public struct Cake {
    public struct Ingredients {
        let mehl: Unit
        let mandelMilch: Unit
        let zucker: Unit
        let oel: Unit                // Am Besten Rapsoel, oder etwas anderes Geschmacksneutrales
        let backPulver: Unit
        let vanilleZucker: Unit      // Die Betonung liegt auf "Vanille"(!) und nicht "Vanillin"!
        let salz: Unit
        let apfelmus: Unit
        let kakaoPulver: Unit

        init(mehl: Unit, mandelMilch: Unit, zucker: Unit, oel: Unit, backPulver: Unit, vanilleZucker: Unit, salz: Unit, apfelmus: Unit, kakaoPulver: Unit) {
            self.mehl = mehl
            self.mandelMilch = mandelMilch
            self.zucker = zucker
            self.oel = oel
            self.backPulver = backPulver
            self.vanilleZucker = vanilleZucker
            self.salz = salz
            self.apfelmus = apfelmus
            self.kakaoPulver = kakaoPulver
        }
    }

    let name: String
    let ingredients: Ingredients

    init(name: String, ingredients: Ingredients) {
        self.name = name
        self.ingredients = ingredients
    }

    func letItGo() {
        Task {
            prepareOven(temperature: 180)
            await listIngredients()
            await prepareIngredients()
        }
    }
}

// MARK: - Public API

extension Cake: Preparation {
    internal func listIngredients() async {
        await wait(seconds: 3)
        print(name.uppercased())
        print(String(repeating: "=", count: name.count))
        print("")
        print("👉🏻 Folgende Zutaten werden gebraucht:")
        print("   → \(ingredients.mehl.quantity) Mehl")
        print("   → \(ingredients.mandelMilch.quantity) Mandelmilch")
        print("   → \(ingredients.zucker.quantity) Zucker")
        print("   → \(ingredients.oel.quantity) Öl")
        print("   → \(ingredients.backPulver.quantity) Backpulver")
        print("   → \(ingredients.vanilleZucker.quantity) Vanillezucker")
        print("   → \(ingredients.salz.quantity) Salz")
        print("   → \(ingredients.apfelmus.quantity) Apfelmus")
        print("   → \(ingredients.kakaoPulver.quantity) Kakaopulver")
        print("")
        await wait(seconds: 3)
    }

    internal func prepareIngredients() async {
        print("👉🏻 🥣 Gib alle Zutaten - AUSSER den Kakao!! - in eine Schüssel, und verrühre sie zu einer cremigen Masse.")
        await wait(seconds: 5)
        print("   🧈 Fette eine Gugelhupf-Form mit Butter oder Margarine ein.")
        await wait(seconds: 5)
        print("   ½  Teile die Teigmasse auf zwei Schüsseln auf. Ein Teil bleibt hell, zu dem anderen gibst du den Kakao 🥣.")
        await wait(seconds: 4)
        print("👉🏻    Nun kommt der Teig in die gefettete Backform. Je nach Belieben erst der helle Teig oder erst der Schokoteig.")
        await wait(seconds: 4)
        print("      Mit einer Gabel kann man noch in den Teig eintauchen und mit drehenden Bewegungen, die Gabel langsam nach oben")
        print("      und unten bewegend den Teil vorsichtig 'verquirlen'. Das ergibt im fergigen Kuchen ein schönes Muster.")
        await wait(seconds: 5)
        print("⏲️    Der Kuchen sollte ca. 35 Minuten im Ofen bleiben (je nach Backofen).")
        print("      Nach dieser Zeit am besten die 'Stechprobe' machen.")
        await wait(seconds: 3)
        print("      Ist er gut: Dann raus damit und abkühlen lassen.")
        await wait(seconds: 2)
        print("")
        print("🎉    Tattaaa... dein \(name) ist fertig. 😃")
    }

    internal func prepareOven(temperature: Int, circulatingAir: Bool = false) {
        print("👉🏻 🌡️ Heize den Backofen auf \(temperature) Grad vor.")
        print("   🥵 Umluft ist: \(circulatingAir ? "eingeschaltet" : "ausgeschaltet!")")
        print("")

        DispatchQueue.main.asyncAfter(deadline: .now() + 23) {
            print("---")
            print("🔔 Backofen ist heiss!")
            print("---")

            fillOven(with: circulatingAir)
        }
    }

    internal func fillOven(with circulatingAir: Bool) {
        print("👉🏻    So, jetzt sollte der ganze Quatsch in den Backofen.")
        print("      Und wie gesagt, die Umluft ist \(circulatingAir ? "eingeschaltet" : "ausgeschaltet!")")
    }
}

// MARK: - Private API

internal extension Cake {
    func wait(seconds: Double) async {
        do {
            try await Task.sleep(nanoseconds: UInt64(seconds * Double(NSEC_PER_SEC)))
        } catch {
            print("** AN ERROR HAS OCCURED! **")
        }
    }
}


// MARK: - Here we go...

let ingredients = Cake.Ingredients(
    mehl: .tasse(2),
    mandelMilch: .tasse(1),
    zucker: .tasse(1),
    oel: .tasse(0.5),
    backPulver: .paeckchen(1),
    vanilleZucker: .paeckchen(1),
    salz: .prise(1),
    apfelmus: .gramm(120),
    kakaoPulver: .essloeffel(2)
)

let cake = Cake(name: "Veganer Tassen-Gugelhupf", ingredients: ingredients)
cake.letItGo()
```
