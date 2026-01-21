# C# Windows Forms - Cheat Sheet pentru Concurs

---

## Partea 1: Cum Funcționează C# în Spatele Scenei

### Imaginea de Ansamblu

```
Codul tău (.cs) → Compiler → IL Code (.exe/.dll) → CLR → Machine Code → CPU
```

**Analogie:** Gândește-te ca la o carte de rețete.
- Tu scrii rețetele în română (codul C#)
- Un traducător le convertește într-un limbaj universal de gătit (IL - Intermediate Language)
- Un bucătar din fiecare bucătărie (CLR - Common Language Runtime) citește acel limbaj universal și gătește efectiv (execută) mâncarea folosind echipamentul local (CPU)

### Componente Cheie (Key Components)

| Componentă | Ce Face | Analogie |
|------------|---------|----------|
| **C# Compiler** | Convertește codul tău în IL | Traducător |
| **IL (Intermediate Language)** | Bytecode universal .NET | Limbaj universal de rețete |
| **CLR (Common Language Runtime)** | Execută IL, gestionează memoria | Bucătarul |
| **JIT (Just-In-Time Compiler)** | Convertește IL în cod mașină la runtime | Bucătar care citește rețeta în timp ce gătește |
| **Garbage Collector** | Eliberează automat memoria nefolosită | Mașina de spălat vase care curăță |

### Memorie: Stack vs Heap

```
STACK (rapid, automat)           HEAP (flexibil, gestionat)
┌─────────────────────┐          ┌─────────────────────┐
│ int x = 5;          │          │ ┌─────────────────┐ │
│ bool flag = true;   │          │ │ Obiect Student  │ │
│ referință ──────────┼──────────┼─▶│ Nume: "Alex"   │ │
│                     │          │ │ Varsta: 16     │ │
└─────────────────────┘          │ └─────────────────┘ │
                                 └─────────────────────┘
```

**Analogie:**
- **Stack** = Biroul tău. Mic, organizat, lucrurile vin și pleacă rapid. Variabilele locale stau aici.
- **Heap** = Un depozit. Mare, flexibil, stochează obiecte. Garbage collector-ul îl curăță.

---

## Partea 2: Fundamentele C# (C# Fundamentals)

### Variabile și Tipuri de Date (Variables and Data Types)

```csharp
// Value types (tipuri valoare - stocate pe stack, conțin date efective)
int varsta = 16;                 // numere întregi (whole numbers)
double pret = 19.99;             // zecimale (precise)
float temperatura = 98.6f;       // zecimale (mai puțin precise, notă 'f')
bool esteStudent = true;         // adevărat sau fals (true or false)
char nota = 'A';                 // un singur caracter (single character)
decimal bani = 1000.50m;         // bani (cel mai precis, notă 'm')

// Reference types (tipuri referință - stocate pe heap, variabila ține adresa)
string nume = "Alex";            // text
int[] scoruri = {90, 85, 92};    // array (matrice)
Student s = new Student();       // obiect (object)
```

**Analogie:**
- Tipurile valoare sunt ca scrierea unui număr pe un post-it — notița ESTE valoarea
- Tipurile referință sunt ca scrierea adresei cuiva — notița arată spre unde locuiește

### Conversie de Tip (Type Conversion)

```csharp
// Implicit (automată, sigură - de la mic la mare)
int x = 10;
double y = x;  // int încape în double, automat

// Explicit (manuală, riscantă - de la mare la mic)
double a = 10.7;
int b = (int)a;  // b = 10, partea zecimală se pierde

// Parsarea string-urilor (Parsing strings)
string input = "42";
int numar = int.Parse(input);           // crash dacă e invalid
int numar2 = Convert.ToInt32(input);    // tot crash dacă e invalid

// Parsare sigură (recomandat pentru input de la utilizator)
if (int.TryParse(input, out int rezultat))
{
    // rezultat conține acum 42
}
else
{
    // input nu era un număr valid
}
```

### Operatori (Operators)

```csharp
// Aritmetici (Arithmetic)
+  -  *  /  %    // adunare, scădere, înmulțire, împărțire, rest (remainder)
++  --           // incrementare, decrementare

// Comparație (Comparison) - returnează bool
==  !=           // egal, diferit (equal, not equal)
<   >            // mai mic, mai mare (less than, greater than)
<=  >=           // mai mic/mare sau egal

// Logici (Logical)
&&               // ȘI (AND) - ambele trebuie să fie true
||               // SAU (OR) - cel puțin unul trebuie să fie true
!                // NU (NOT) - inversează true/false

// Scurtături de atribuire (Assignment shortcuts)
x += 5;          // echivalent cu x = x + 5
x -= 3;          // echivalent cu x = x - 3
x *= 2;          // echivalent cu x = x * 2
```

### String-uri (Strings)

```csharp
string nume = "Alex";

// Operații comune (Common operations)
nume.Length              // 4 (lungime)
nume.ToUpper()           // "ALEX" (majuscule)
nume.ToLower()           // "alex" (minuscule)
nume.Contains("le")      // true (conține)
nume.StartsWith("A")     // true (începe cu)
nume.Substring(1, 2)     // "le" (pornește de la index 1, ia 2 caractere)
nume.Replace("A", "E")   // "Elex" (înlocuiește)
nume.Split(',')          // împarte după virgulă într-un array

// Concatenare string-uri (String concatenation)
string complet = "Salut " + nume;                 // basic
string complet2 = $"Salut {nume}";                // interpolation (preferat)
string complet3 = string.Format("Salut {0}", nume);

// Compararea string-urilor (Comparing strings)
string a = "salut";
string b = "Salut";
a == b                                            // false (case sensitive)
a.Equals(b, StringComparison.OrdinalIgnoreCase)   // true (ignoră majuscule)
```

---

## Partea 3: Control Flow (Fluxul de Control)

### If/Else

```csharp
int scor = 85;

if (scor >= 90)
{
    Console.WriteLine("A");
}
else if (scor >= 80)
{
    Console.WriteLine("B");
}
else if (scor >= 70)
{
    Console.WriteLine("C");
}
else
{
    Console.WriteLine("F");
}

// Ternary operator (prescurtare pentru if/else simplu)
string rezultat = scor >= 60 ? "Trecut" : "Picat";
```

### Switch

```csharp
string zi = "Luni";

switch (zi)
{
    case "Luni":
    case "Marți":
    case "Miercuri":
    case "Joi":
    case "Vineri":
        Console.WriteLine("Zi lucrătoare");
        break;
    case "Sâmbătă":
    case "Duminică":
        Console.WriteLine("Weekend");
        break;
    default:
        Console.WriteLine("Zi invalidă");
        break;
}
```

### Bucle (Loops)

```csharp
// For loop - când știi de câte ori
for (int i = 0; i < 5; i++)
{
    Console.WriteLine(i);  // 0, 1, 2, 3, 4
}

// While loop - când nu știi de câte ori
int contor = 0;
while (contor < 5)
{
    Console.WriteLine(contor);
    contor++;
}

// Do-while - rulează cel puțin o dată
do
{
    Console.WriteLine("Rulează cel puțin o dată");
} while (false);

// Foreach - pentru colecții
string[] nume = {"Alex", "Sam", "Jordan"};
foreach (string n in nume)
{
    Console.WriteLine(n);
}

// Controlul buclelor (Loop control)
break;      // ieși din buclă imediat
continue;   // sari la următoarea iterație
```

**Analogie:**
- `for` loop = "Fă asta exact de 10 ori"
- `while` loop = "Continuă să faci asta până zic stop"
- `foreach` loop = "Fă asta pentru fiecare element din cutie"

---

## Partea 4: Colecții (Collections)

### Arrays (Matrice)

```csharp
// Dimensiune fixă, nu poate crește sau scădea
int[] numere = new int[5];            // array gol de 5 elemente
int[] scoruri = {90, 85, 92, 88, 95}; // array inițializat

scoruri[0]        // 90 (primul element)
scoruri[4]        // 95 (ultimul element)
scoruri.Length    // 5

// Array 2D
int[,] grid = new int[3, 3];  // grilă 3x3
grid[0, 0] = 1;
grid[1, 2] = 5;
```

### Lists (Liste)

```csharp
using System.Collections.Generic;

// Dimensiune dinamică, poate crește și scădea
List<string> nume = new List<string>();
nume.Add("Alex");
nume.Add("Sam");
nume.Insert(0, "Jordan");  // inserează la indexul 0

nume[0]               // "Jordan"
nume.Count            // 3 (nu Length!)
nume.Contains("Sam")  // true
nume.Remove("Sam");   // elimină primul "Sam"
nume.RemoveAt(0);     // elimină elementul de la indexul 0
nume.Clear();         // elimină tot

// Inițializare cu valori
List<int> nums = new List<int> {1, 2, 3, 4, 5};
```

**Analogie:**
- Array = Un rând de cutii poștale. Număr fix, nu poți adăuga mai multe.
- List = O pungă extensibilă. Adaugi sau elimini oricând.

### Dictionary (Dicționar)

```csharp
// Perechi cheie-valoare (key-value pairs), ca un dicționar real
Dictionary<string, int> varste = new Dictionary<string, int>();
varste["Alex"] = 16;
varste["Sam"] = 17;
varste.Add("Jordan", 15);  // altă metodă de adăugare

int varstaAlex = varste["Alex"];           // 16
bool areCheia = varste.ContainsKey("Alex"); // true
varste.Remove("Sam");

// Parcurgere (Loop through)
foreach (var pereche in varste)
{
    Console.WriteLine($"{pereche.Key}: {pereche.Value}");
}

// Acces sigur (Safe access)
if (varste.TryGetValue("Necunoscut", out int varsta))
{
    // folosește varsta
}
```

**Analogie:** Un dicționar e ca o carte de telefon — cauți un nume (key) pentru a găsi un număr (value).

---

## Partea 5: Metode (Methods / Functions)

### Metode de Bază (Basic Methods)

```csharp
// Metodă care nu returnează nimic
void SpuneSalut()
{
    Console.WriteLine("Salut!");
}

// Metodă cu parametri
void Saluta(string nume)
{
    Console.WriteLine($"Salut, {nume}!");
}

// Metodă care returnează o valoare
int Aduna(int a, int b)
{
    return a + b;
}

// Metodă cu parametru implicit (default parameter)
void Log(string mesaj, bool urgent = false)
{
    if (urgent) Console.WriteLine("URGENT: " + mesaj);
    else Console.WriteLine(mesaj);
}
Log("Test");           // folosește default (false)
Log("Eroare!", true);  // trece true
```

### Parametri Out și Ref

```csharp
// out - metoda TREBUIE să atribuie valoare, apelantul nu trebuie să inițializeze
void GetValori(out int x, out int y)
{
    x = 10;
    y = 20;
}
GetValori(out int a, out int b);  // a=10, b=20

// ref - apelantul TREBUIE să inițializeze, metoda poate modifica
void Dubleaza(ref int numar)
{
    numar *= 2;
}
int val = 5;
Dubleaza(ref val);  // val este acum 10
```

### Method Overloading (Supraîncărcarea Metodelor)

```csharp
// Același nume, parametri diferiți
int Aduna(int a, int b) { return a + b; }
double Aduna(double a, double b) { return a + b; }
int Aduna(int a, int b, int c) { return a + b + c; }

Aduna(1, 2);        // apelează prima versiune
Aduna(1.5, 2.5);    // apelează a doua versiune
Aduna(1, 2, 3);     // apelează a treia versiune
```

**Analogie:** Method overloading e ca un restaurant care ia comenzi — "Vreau burger" vs "Vreau burger cu cartofi" vs "Vreau burger cu cartofi și băutură." Aceeași comandă de bază, variații diferite.

---

## Partea 6: Programare Orientată pe Obiecte (OOP)

### Clase și Obiecte (Classes and Objects)

```csharp
// Class = Blueprint (Șablon)
public class Student
{
    // Fields (câmpuri - private - date interne)
    private string nume;
    private int varsta;
    private double medie;
    
    // Properties (proprietăți - public - acces controlat)
    public string Nume
    {
        get { return nume; }
        set { nume = value; }
    }
    
    // Auto-property (prescurtare)
    public int Varsta { get; set; }
    public double Medie { get; private set; }  // read-only din exterior
    
    // Constructor (rulează când obiectul e creat)
    public Student(string nume, int varsta)
    {
        this.nume = nume;
        this.Varsta = varsta;
        this.medie = 0.0;
    }
    
    // Method (Metodă)
    public void Studiaza(int ore)
    {
        medie += ore * 0.1;
        if (medie > 10.0) medie = 10.0;
    }
    
    // Override ToString pentru afișare
    public override string ToString()
    {
        return $"{Nume}, Varsta {Varsta}, Medie: {Medie:F2}";
    }
}

// Object = Instanță a șablonului
Student alex = new Student("Alex", 16);
alex.Varsta = 17;          // folosind property
alex.Studiaza(5);          // apelând metodă
Console.WriteLine(alex);   // folosește ToString
```

**Analogie:**
- Class = Forma de prăjituri (șablon/template)
- Object = Prăjitura efectivă (făcută cu forma)
- Constructor = Momentul când apeși forma în aluat
- Properties = Ferestre controlate pentru a vedea sau schimba lucruri
- Methods = Lucruri pe care prăjitura le poate face

### Access Modifiers (Modificatori de Acces)

```csharp
public      // oricine poate accesa
private     // doar această clasă poate accesa
protected   // această clasă și copiii pot accesa
internal    // doar acest proiect poate accesa
```

**Analogie:**
- `public` = Ușa din față, oricine poate intra
- `private` = Dormitorul tău, doar tu
- `protected` = Camera de familie, tu și copiii tăi
- `internal` = Clădirea companiei, doar angajații

### Cei Patru Piloni ai OOP (The Four Pillars of OOP)

#### 1. Encapsulation (Încapsulare)

Ascunderea detaliilor interne, expunând doar ce e necesar.

```csharp
public class ContBancar
{
    private decimal sold;  // ascuns (hidden)
    
    public decimal Sold    // acces controlat
    {
        get { return sold; }
    }
    
    public void Depune(decimal suma)
    {
        if (suma > 0)
            sold += suma;
    }
    
    public bool Retrage(decimal suma)
    {
        if (suma > 0 && suma <= sold)
        {
            sold -= suma;
            return true;
        }
        return false;
    }
}
```

**Analogie:** Un ATM. Nu poți băga mâna și lua bani — folosești butoane (metode) care controlează ce se întâmplă.

#### 2. Inheritance (Moștenire)

Crearea de clase noi bazate pe cele existente.

```csharp
// Base class (clasă părinte)
public class Animal
{
    public string Nume { get; set; }
    
    public virtual void Vorbeste()  // virtual = poate fi suprascris
    {
        Console.WriteLine("Un sunet");
    }
}

// Derived class (clasă copil)
public class Caine : Animal
{
    public string Rasa { get; set; }
    
    public override void Vorbeste()  // suprascrie metoda părintelui
    {
        Console.WriteLine("Ham!");
    }
    
    public void AduMingea()
    {
        Console.WriteLine($"{Nume} aduce mingea!");
    }
}

Caine buddy = new Caine();
buddy.Nume = "Buddy";   // moștenit de la Animal
buddy.Rasa = "Labrador"; // proprietate proprie a Caine
buddy.Vorbeste();       // "Ham!" (suprascris)
buddy.AduMingea();      // metodă proprie a Caine
```

**Analogie:** Un copil moștenește trăsături de la părinți dar poate avea și trăsături proprii unice.

#### 3. Polymorphism (Polimorfism)

Tratarea obiectelor diferite în același mod printr-o interfață comună.

```csharp
public class Pisica : Animal
{
    public override void Vorbeste()
    {
        Console.WriteLine("Miau!");
    }
}

// Polimorfism în acțiune
List<Animal> animale = new List<Animal>();
animale.Add(new Caine { Nume = "Buddy" });
animale.Add(new Pisica { Nume = "Whiskers" });

foreach (Animal animal in animale)
{
    Console.Write($"{animal.Nume}: ");
    animal.Vorbeste();  // Fiecare vorbește diferit!
}
// Output:
// Buddy: Ham!
// Whiskers: Miau!
```

**Analogie:** O telecomandă universală. Apeși "play" și fiecare dispozitiv face propria versiune de play.

#### 4. Abstraction (Abstractizare)

Ascunderea complexității, arătând doar esențialul.

```csharp
// Abstract class - nu poate fi instanțiată direct
public abstract class Forma
{
    public abstract double Arie();        // trebuie implementat
    public abstract double Perimetru();   // trebuie implementat
    
    public void Afiseaza()  // metodă concretă
    {
        Console.WriteLine($"Arie: {Arie()}, Perimetru: {Perimetru()}");
    }
}

public class Dreptunghi : Forma
{
    public double Latime { get; set; }
    public double Inaltime { get; set; }
    
    public override double Arie() => Latime * Inaltime;
    public override double Perimetru() => 2 * (Latime + Inaltime);
}

public class Cerc : Forma
{
    public double Raza { get; set; }
    
    public override double Arie() => Math.PI * Raza * Raza;
    public override double Perimetru() => 2 * Math.PI * Raza;
}
```

**Analogie:** Volanul mașinii. Îl rotești să mergi stânga/dreapta fără să știi cum funcționează mecanismul de direcție.

### Interfaces (Interfețe)

Un contract pe care clasele trebuie să-l respecte.

```csharp
// Interface - definește CE, nu CUM
public interface ISalvabil
{
    void Salveaza(string numeFisier);
    void Incarca(string numeFisier);
}

public interface IPrintabil
{
    void Printeaza();
}

// Clasa poate implementa mai multe interfețe
public class Document : ISalvabil, IPrintabil
{
    public string Continut { get; set; }
    
    public void Salveaza(string numeFisier)
    {
        File.WriteAllText(numeFisier, Continut);
    }
    
    public void Incarca(string numeFisier)
    {
        Continut = File.ReadAllText(numeFisier);
    }
    
    public void Printeaza()
    {
        Console.WriteLine(Continut);
    }
}
```

**Analogie:** O interfață e ca o cerință de job. "Trebuie să știi să conduci" nu spune ce mașină — doar că poți conduce.

---

## Partea 7: Arhitectura Windows Forms

### Cum Funcționează Windows Forms

```
┌─────────────────────────────────────────────────────────────┐
│                    Aplicația Ta                             │
├─────────────────────────────────────────────────────────────┤
│  Form1.cs (codul tău)     │    Form1.Designer.cs (auto)    │
│  - Event handlers         │    - Definiții controale       │
│  - Business logic         │    - Setup layout              │
├─────────────────────────────────────────────────────────────┤
│                Windows Forms Framework                       │
│         (gestionează desenare, events, message loop)        │
├─────────────────────────────────────────────────────────────┤
│                    Windows OS                                │
└─────────────────────────────────────────────────────────────┘
```

### Message Loop (Bucla de Mesaje)

Când aplicația rulează, Windows Forms intră într-o buclă de mesaje:

```
┌──────────────┐
│ Așteaptă     │ ◄────────────────────────────┐
│ acțiune user │                              │
└──────┬───────┘                              │
       ▼                                      │
┌──────────────┐                              │
│ Windows      │                              │
│ trimite mesaj│                              │
└──────┬───────┘                              │
       ▼                                      │
┌──────────────┐                              │
│ Event        │                              │
│ handler-ul   │                              │
│ tău rulează  │                              │
└──────┬───────┘                              │
       ▼                                      │
┌──────────────┐                              │
│ UI se        │──────────────────────────────┘
│ actualizează │
└──────────────┘
```

**Analogie:** O recepționeră la birou. Așteaptă vizitatori (events), apoi sună persoana potrivită (event handler) să se ocupe de fiecare vizitator.

### Structura Formularului (Form Structure)

```csharp
// Form1.cs - CODUL TĂU
public partial class Form1 : Form
{
    public Form1()
    {
        InitializeComponent();  // apelează Designer.cs
        // Codul tău de inițializare aici
    }
    
    private void button1_Click(object sender, EventArgs e)
    {
        // Codul event handler-ului tău
    }
}

// Form1.Designer.cs - AUTO-GENERAT (nu edita direct)
partial class Form1
{
    private void InitializeComponent()
    {
        this.button1 = new Button();
        this.button1.Location = new Point(100, 50);
        this.button1.Text = "Click Me";
        this.button1.Click += button1_Click;  // conectează event-ul
        this.Controls.Add(this.button1);
    }
    
    private Button button1;
}
```

### Controale Comune (Common Controls)

| Control | Scop | Proprietăți Cheie | Events Cheie |
|---------|------|-------------------|--------------|
| **Label** | Afișare text | Text, Font, ForeColor | Click |
| **TextBox** | Input text | Text, Multiline, ReadOnly | TextChanged, KeyPress |
| **Button** | Declanșare acțiune | Text, Enabled | Click |
| **ComboBox** | Listă dropdown | Items, SelectedItem, SelectedIndex | SelectedIndexChanged |
| **ListBox** | Selecție din listă | Items, SelectedItem, SelectionMode | SelectedIndexChanged |
| **CheckBox** | Opțiune da/nu | Checked, Text | CheckedChanged |
| **RadioButton** | Una din mai multe | Checked, Text, GroupName | CheckedChanged |
| **DataGridView** | Afișare tabel | DataSource, Columns, Rows | CellClick, CellValueChanged |
| **PictureBox** | Afișare imagine | Image, SizeMode | Click |
| **DateTimePicker** | Selecție dată | Value, Format | ValueChanged |
| **NumericUpDown** | Input numeric | Value, Minimum, Maximum | ValueChanged |
| **MenuStrip** | Bară meniu | Items | Click (pe items) |
| **Timer** | Events temporizate | Interval, Enabled | Tick |

### Lucrul cu Controale (Working with Controls)

```csharp
// Citirea valorilor (Reading values)
string text = textBox1.Text;
int index = comboBox1.SelectedIndex;
string selectat = comboBox1.SelectedItem?.ToString();
bool bifat = checkBox1.Checked;
DateTime data = dateTimePicker1.Value;
decimal numar = numericUpDown1.Value;

// Setarea valorilor (Setting values)
label1.Text = "Salut!";
textBox1.Text = "Text implicit";
comboBox1.SelectedIndex = 0;
checkBox1.Checked = true;
button1.Enabled = false;  // gri (dezactivat)

// Adăugarea de elemente în liste
comboBox1.Items.Add("Opțiunea 1");
comboBox1.Items.AddRange(new string[] {"A", "B", "C"});
listBox1.Items.Clear();

// Stilizare (Styling)
label1.ForeColor = Color.Red;
label1.BackColor = Color.Yellow;
label1.Font = new Font("Arial", 12, FontStyle.Bold);
```

### Events (Evenimente)

```csharp
// Button click
private void button1_Click(object sender, EventArgs e)
{
    MessageBox.Show("Click!");
}

// Text changed
private void textBox1_TextChanged(object sender, EventArgs e)
{
    label1.Text = textBox1.Text.Length + " caractere";
}

// Form loading
private void Form1_Load(object sender, EventArgs e)
{
    // Inițializează datele când se deschide formularul
    IncarcaDate();
}

// Form closing (cu opțiune de anulare)
private void Form1_FormClosing(object sender, FormClosingEventArgs e)
{
    var rezultat = MessageBox.Show("Salvezi înainte de ieșire?", "Confirmare",
        MessageBoxButtons.YesNoCancel);
    
    if (rezultat == DialogResult.Cancel)
        e.Cancel = true;  // previne închiderea
    else if (rezultat == DialogResult.Yes)
        SalveazaDate();
}

// Key press (pentru validare input)
private void textBox1_KeyPress(object sender, KeyPressEventArgs e)
{
    // Permite doar cifre
    if (!char.IsDigit(e.KeyChar) && e.KeyChar != (char)Keys.Back)
    {
        e.Handled = true;  // blochează tasta
    }
}

// Timer tick
private void timer1_Tick(object sender, EventArgs e)
{
    label1.Text = DateTime.Now.ToString("HH:mm:ss");
}
```

### Lucrul cu Mai Multe Formulare (Working with Multiple Forms)

```csharp
// Deschiderea unui nou formular
Form2 form2 = new Form2();
form2.Show();           // non-modal (ambele formulare utilizabile)
// SAU
form2.ShowDialog();     // modal (trebuie să închizi Form2 întâi)

// Trimiterea datelor CĂTRE alt formular
public partial class Form2 : Form
{
    public string DateTrimise { get; set; }
    
    private void Form2_Load(object sender, EventArgs e)
    {
        label1.Text = DateTrimise;
    }
}

// În Form1:
Form2 form2 = new Form2();
form2.DateTrimise = "Salut de la Form1!";
form2.ShowDialog();

// Obținerea datelor ÎNAPOI de la alt formular
public partial class Form2 : Form
{
    public string Rezultat { get; private set; }
    
    private void btnOK_Click(object sender, EventArgs e)
    {
        Rezultat = textBox1.Text;
        this.DialogResult = DialogResult.OK;
        this.Close();
    }
}

// În Form1:
Form2 form2 = new Form2();
if (form2.ShowDialog() == DialogResult.OK)
{
    string rezultat = form2.Rezultat;
}
```

### MessageBox

```csharp
// Mesaj simplu
MessageBox.Show("Salut!");

// Cu titlu
MessageBox.Show("Fișier salvat.", "Succes");

// Cu butoane și icon
MessageBox.Show("Ștergi acest element?", "Confirmare",
    MessageBoxButtons.YesNo,
    MessageBoxIcon.Question);

// Verifică rezultatul
DialogResult rezultat = MessageBox.Show("Continui?", "Confirmare",
    MessageBoxButtons.YesNoCancel,
    MessageBoxIcon.Warning);

if (rezultat == DialogResult.Yes)
    // fă ceva
else if (rezultat == DialogResult.No)
    // fă altceva
// Cancel = doar închide dialogul
```

### DataGridView (Tabele)

```csharp
// Binding la o List
List<Student> studenti = new List<Student>();
studenti.Add(new Student { Nume = "Alex", Varsta = 16, Nota = "A" });
studenti.Add(new Student { Nume = "Sam", Varsta = 17, Nota = "B" });

dataGridView1.DataSource = studenti;

// Coloane manuale
dataGridView1.Columns.Clear();
dataGridView1.Columns.Add("Nume", "Numele Studentului");
dataGridView1.Columns.Add("Nota", "Nota");

dataGridView1.Rows.Add("Alex", "A");
dataGridView1.Rows.Add("Sam", "B");

// Citirea rândului selectat
if (dataGridView1.SelectedRows.Count > 0)
{
    DataGridViewRow rand = dataGridView1.SelectedRows[0];
    string nume = rand.Cells["Nume"].Value.ToString();
}

// Cell click event
private void dataGridView1_CellClick(object sender, DataGridViewCellEventArgs e)
{
    if (e.RowIndex >= 0)
    {
        DataGridViewRow rand = dataGridView1.Rows[e.RowIndex];
        textBox1.Text = rand.Cells[0].Value?.ToString();
    }
}
```

---

## Partea 8: File I/O (Salvare/Încărcare Date)

### Fișiere Text Simple

```csharp
using System.IO;

// Scrie tot textul dintr-o dată
File.WriteAllText("date.txt", "Salut Lume");

// Scrie mai multe linii
string[] linii = {"Linia 1", "Linia 2", "Linia 3"};
File.WriteAllLines("date.txt", linii);

// Citește tot textul
string continut = File.ReadAllText("date.txt");

// Citește toate liniile într-un array
string[] liniiCitite = File.ReadAllLines("date.txt");

// Adaugă la fișier
File.AppendAllText("log.txt", "Intrare nouă\n");

// Verifică dacă fișierul există
if (File.Exists("date.txt"))
{
    // procesează fișierul
}
```

### Fișiere CSV (Comma-Separated Values)

```csharp
// Scriere CSV
List<Student> studenti = GetStudenti();
List<string> linii = new List<string>();
linii.Add("Nume,Varsta,Nota");  // header

foreach (var s in studenti)
{
    linii.Add($"{s.Nume},{s.Varsta},{s.Nota}");
}
File.WriteAllLines("studenti.csv", linii);

// Citire CSV
string[] liniiCSV = File.ReadAllLines("studenti.csv");
List<Student> incarcati = new List<Student>();

for (int i = 1; i < liniiCSV.Length; i++)  // sari peste header
{
    string[] parti = liniiCSV[i].Split(',');
    incarcati.Add(new Student
    {
        Nume = parti[0],
        Varsta = int.Parse(parti[1]),
        Nota = parti[2]
    });
}
```

### JSON Serialization (Recomandat)

```csharp
using System.Text.Json;

// Salvează obiect în JSON
Student student = new Student { Nume = "Alex", Varsta = 16, Nota = "A" };
string json = JsonSerializer.Serialize(student);
File.WriteAllText("student.json", json);
// Fișierul conține: {"Nume":"Alex","Varsta":16,"Nota":"A"}

// Încarcă obiect din JSON
string json = File.ReadAllText("student.json");
Student incarcat = JsonSerializer.Deserialize<Student>(json);

// Salvează List în JSON
List<Student> studenti = GetStudenti();
string jsonList = JsonSerializer.Serialize(studenti);
File.WriteAllText("studenti.json", jsonList);

// Încarcă List din JSON
string jsonList = File.ReadAllText("studenti.json");
List<Student> listaIncarcata = JsonSerializer.Deserialize<List<Student>>(jsonList);

// Pretty print (formatat frumos)
var optiuni = new JsonSerializerOptions { WriteIndented = true };
string frumos = JsonSerializer.Serialize(studenti, optiuni);
```

### File Dialogs (Dialoguri de Fișiere)

```csharp
// Open file dialog
OpenFileDialog dialogDeschide = new OpenFileDialog();
dialogDeschide.Filter = "Fișiere text (*.txt)|*.txt|Toate fișierele (*.*)|*.*";
dialogDeschide.InitialDirectory = Environment.GetFolderPath(Environment.SpecialFolder.MyDocuments);

if (dialogDeschide.ShowDialog() == DialogResult.OK)
{
    string numeFisier = dialogDeschide.FileName;
    string continut = File.ReadAllText(numeFisier);
}

// Save file dialog
SaveFileDialog dialogSalveaza = new SaveFileDialog();
dialogSalveaza.Filter = "Fișiere text (*.txt)|*.txt|Fișiere JSON (*.json)|*.json";
dialogSalveaza.DefaultExt = "txt";

if (dialogSalveaza.ShowDialog() == DialogResult.OK)
{
    File.WriteAllText(dialogSalveaza.FileName, textBox1.Text);
}
```

---

## Partea 9: Error Handling (Gestionarea Erorilor)

### Try-Catch-Finally

```csharp
try
{
    // Cod care ar putea eșua
    int rezultat = int.Parse(textBox1.Text);
    int impartit = 100 / rezultat;
}
catch (FormatException)
{
    // Excepție specifică
    MessageBox.Show("Te rog introdu un număr valid");
}
catch (DivideByZeroException)
{
    MessageBox.Show("Nu se poate împărți la zero");
}
catch (Exception ex)
{
    // Catch-all (pune-l ultimul)
    MessageBox.Show($"Eroare: {ex.Message}");
}
finally
{
    // Rulează întotdeauna (cleanup)
    textBox1.Clear();
}
```

### Input Validation (Validarea Inputului)

```csharp
// Validează înainte de procesare
private void btnSubmit_Click(object sender, EventArgs e)
{
    // Verifică gol
    if (string.IsNullOrWhiteSpace(txtNume.Text))
    {
        MessageBox.Show("Numele este obligatoriu");
        txtNume.Focus();
        return;
    }
    
    // Verifică numeric
    if (!int.TryParse(txtVarsta.Text, out int varsta))
    {
        MessageBox.Show("Vârsta trebuie să fie un număr");
        txtVarsta.Focus();
        return;
    }
    
    // Verifică range
    if (varsta < 0 || varsta > 150)
    {
        MessageBox.Show("Vârsta trebuie să fie între 0 și 150");
        txtVarsta.Focus();
        return;
    }
    
    // Totul valid, continuă
    ProceseazaDate(txtNume.Text, varsta);
}
```

---

## Partea 10: Șabloane Comune pentru Concursuri

### Structură de Bază CRUD Application

```csharp
public partial class MainForm : Form
{
    private List<Student> studenti = new List<Student>();
    private string fisierDate = "studenti.json";
    
    public MainForm()
    {
        InitializeComponent();
    }
    
    private void MainForm_Load(object sender, EventArgs e)
    {
        IncarcaDate();
        RefreshGrid();
    }
    
    // CREATE (Creare)
    private void btnAdauga_Click(object sender, EventArgs e)
    {
        if (!ValideazaInput()) return;
        
        var student = new Student
        {
            Nume = txtNume.Text,
            Varsta = int.Parse(txtVarsta.Text),
            Nota = cboNota.SelectedItem.ToString()
        };
        
        studenti.Add(student);
        SalveazaDate();
        RefreshGrid();
        CurataFormular();
    }
    
    // READ (Afișare)
    private void RefreshGrid()
    {
        dataGridView1.DataSource = null;
        dataGridView1.DataSource = studenti;
    }
    
    // UPDATE (Actualizare)
    private void btnActualizeaza_Click(object sender, EventArgs e)
    {
        if (dataGridView1.SelectedRows.Count == 0)
        {
            MessageBox.Show("Selectează un student întâi");
            return;
        }
        
        int index = dataGridView1.SelectedRows[0].Index;
        studenti[index].Nume = txtNume.Text;
        studenti[index].Varsta = int.Parse(txtVarsta.Text);
        studenti[index].Nota = cboNota.SelectedItem.ToString();
        
        SalveazaDate();
        RefreshGrid();
    }
    
    // DELETE (Ștergere)
    private void btnSterge_Click(object sender, EventArgs e)
    {
        if (dataGridView1.SelectedRows.Count == 0) return;
        
        var rezultat = MessageBox.Show("Ștergi acest student?", "Confirmare",
            MessageBoxButtons.YesNo, MessageBoxIcon.Warning);
        
        if (rezultat == DialogResult.Yes)
        {
            int index = dataGridView1.SelectedRows[0].Index;
            studenti.RemoveAt(index);
            SalveazaDate();
            RefreshGrid();
            CurataFormular();
        }
    }
    
    // SEARCH (Căutare)
    private void txtCautare_TextChanged(object sender, EventArgs e)
    {
        string cautare = txtCautare.Text.ToLower();
        var filtrati = studenti.Where(s => 
            s.Nume.ToLower().Contains(cautare)).ToList();
        
        dataGridView1.DataSource = null;
        dataGridView1.DataSource = filtrati;
    }
    
    // Metode ajutătoare (Helper methods)
    private void IncarcaDate()
    {
        if (File.Exists(fisierDate))
        {
            string json = File.ReadAllText(fisierDate);
            studenti = JsonSerializer.Deserialize<List<Student>>(json) 
                       ?? new List<Student>();
        }
    }
    
    private void SalveazaDate()
    {
        string json = JsonSerializer.Serialize(studenti);
        File.WriteAllText(fisierDate, json);
    }
    
    private void CurataFormular()
    {
        txtNume.Clear();
        txtVarsta.Clear();
        cboNota.SelectedIndex = -1;
        txtNume.Focus();
    }
    
    private bool ValideazaInput()
    {
        if (string.IsNullOrWhiteSpace(txtNume.Text))
        {
            MessageBox.Show("Numele este obligatoriu");
            return false;
        }
        if (!int.TryParse(txtVarsta.Text, out _))
        {
            MessageBox.Show("Vârsta validă este obligatorie");
            return false;
        }
        if (cboNota.SelectedIndex < 0)
        {
            MessageBox.Show("Selectează o notă");
            return false;
        }
        return true;
    }
    
    // Încarcă rândul selectat în formular
    private void dataGridView1_SelectionChanged(object sender, EventArgs e)
    {
        if (dataGridView1.SelectedRows.Count > 0)
        {
            var rand = dataGridView1.SelectedRows[0];
            txtNume.Text = rand.Cells["Nume"].Value?.ToString();
            txtVarsta.Text = rand.Cells["Varsta"].Value?.ToString();
            cboNota.SelectedItem = rand.Cells["Nota"].Value?.ToString();
        }
    }
}
```

### Formular de Login Simplu

```csharp
public partial class LoginForm : Form
{
    public bool LoginReusit { get; private set; }
    
    private void btnLogin_Click(object sender, EventArgs e)
    {
        string utilizator = txtUtilizator.Text;
        string parola = txtParola.Text;
        
        // Validare simplă (în aplicații reale, niciodată nu stoca parole plain!)
        if (utilizator == "admin" && parola == "parola")
        {
            LoginReusit = true;
            this.Close();
        }
        else
        {
            MessageBox.Show("Credențiale invalide");
            txtParola.Clear();
            txtParola.Focus();
        }
    }
}

// În Program.cs
static void Main()
{
    Application.EnableVisualStyles();
    
    LoginForm login = new LoginForm();
    login.ShowDialog();
    
    if (login.LoginReusit)
    {
        Application.Run(new MainForm());
    }
}
```

---

## Partea 11: Referință Rapidă - Taskuri Comune

### Formatare Numerică (Numeric Formatting)

```csharp
double valoare = 1234.5678;

valoare.ToString("F2")      // "1234.57" (2 zecimale)
valoare.ToString("N2")      // "1,234.57" (cu separator de mii)
valoare.ToString("C")       // "$1,234.57" (monedă)
valoare.ToString("P1")      // "123,456.8%" (procent)

// În string interpolation
$"{valoare:F2}"             // la fel ca ToString("F2")
$"{valoare:C}"              // monedă
```

### Date/Time (Dată/Oră)

```csharp
DateTime acum = DateTime.Now;
DateTime astazi = DateTime.Today;
DateTime specifica = new DateTime(2024, 12, 25);

// Formatare
acum.ToString("yyyy-MM-dd")        // "2024-01-15"
acum.ToString("dd/MM/yyyy")        // "15/01/2024"
acum.ToString("HH:mm:ss")          // "14:30:45" (24-ore)
acum.ToString("hh:mm tt")          // "02:30 PM" (12-ore)
acum.ToString("MMMM dd, yyyy")     // "January 15, 2024"

// Operații
acum.AddDays(7)                    // o săptămână mai târziu
acum.AddMonths(1)                  // o lună mai târziu
(data2 - data1).TotalDays          // zile între date
```

### LINQ Basics (pentru filtrare/sortare)

```csharp
using System.Linq;

List<Student> studenti = GetStudenti();

// Filtrare (Filter)
var premianți = studenti.Where(s => s.Medie >= 9.5).ToList();

// Sortare (Sort)
var sortati = studenti.OrderBy(s => s.Nume).ToList();
var sortatiDesc = studenti.OrderByDescending(s => s.Medie).ToList();

// Găsește unul (Find one)
var alex = studenti.FirstOrDefault(s => s.Nume == "Alex");

// Verifică dacă există vreo potrivire
bool arePremianți = studenti.Any(s => s.Medie >= 9.5);

// Numără potrivirile
int nrPremianți = studenti.Count(s => s.Medie >= 9.5);

// Obține o proprietate specifică
var nume = studenti.Select(s => s.Nume).ToList();

// Înlănțuire operații (Chain operations)
var rezultat = studenti
    .Where(s => s.Varsta >= 16)
    .OrderBy(s => s.Nume)
    .ToList();
```

---

## Partea 12: Sfaturi pentru Concurs

### Înainte Să Scrii Cod

1. **Citește cerințele cu atenție** - de două ori
2. **Planifică clasele întâi** - ce date ai nevoie?
3. **Schițează UI-ul** - ce formulare, ce controale?
4. **Gândește-te la formatul fișierului** - JSON e cel mai ușor

### Greșeli Comune de Evitat

```csharp
// ❌ Nu verifici pentru null
string nume = listBox1.SelectedItem.ToString();  // crash dacă nimic selectat

// ✅ Verifică întâi
if (listBox1.SelectedItem != null)
{
    string nume = listBox1.SelectedItem.ToString();
}

// ❌ Nu validezi inputul utilizatorului
int varsta = int.Parse(textBox1.Text);  // crash pe input invalid

// ✅ Folosește TryParse
if (int.TryParse(textBox1.Text, out int varsta))
{
    // folosește varsta
}

// ❌ Uiți să reîmprospătezi afișarea după schimbări
studenti.Add(studentNou);
// UI-ul tot arată datele vechi!

// ✅ Reîmprospătează afișarea
studenti.Add(studentNou);
dataGridView1.DataSource = null;
dataGridView1.DataSource = studenti;

// ❌ Nu salvezi datele
studenti.Add(studentNou);
// Pierdut când aplicația se închide!

// ✅ Salvează după schimbări
studenti.Add(studentNou);
SalveazaDate();
```

### Checklist Rapid UI

- [ ] Toate butoanele au text clar
- [ ] Câmpurile de input au etichete (labels)
- [ ] Tab order are sens (View → Tab Order)
- [ ] Validare corespunzătoare a inputului
- [ ] Mesajele de eroare sunt utile
- [ ] Confirmă înainte de ștergere
- [ ] Datele persistă după închidere

### Gestionarea Timpului

1. **Primele 20%**: UI de bază funcțional, formularele se deschid
2. **Următoarele 40%**: Funcționalitate de bază (adaugă, afișează, salvează/încarcă)
3. **Următoarele 25%**: Editare, ștergere, căutare
4. **Ultimele 15%**: Polish, gestionare erori, cazuri limită

### Dacă Te Blochezi

1. Compilează? Repară erorile întâi
2. Adaugă `MessageBox.Show("Am ajuns aici")` pentru a urmări execuția
3. Verifică dacă valorile sunt ce te aștepți
4. Caută pe Google mesajul exact de eroare

---

## Card de Referință Rapidă Sintaxă

```csharp
// Variabile
int x = 5;
string s = "salut";
bool b = true;
List<int> lista = new List<int>();
Dictionary<string, int> dict = new Dictionary<string, int>();

// Control flow
if (x > 0) { } else if (x < 0) { } else { }
for (int i = 0; i < 10; i++) { }
foreach (var item in lista) { }
while (conditie) { }
switch (x) { case 1: break; default: break; }

// Metode
void FaCeva() { }
int Aduna(int a, int b) { return a + b; }
string Saluta(string nume = "Lume") { return $"Salut {nume}"; }

// Clase
public class ClasaMea
{
    public string Nume { get; set; }
    public ClasaMea(string nume) { Nume = nume; }
    public void FaCeva() { }
}

// Inheritance
public class Copil : Parinte { }
public class ClasaMea : IInterfataMea { }

// File I/O
File.WriteAllText("fisier.txt", continut);
string continut = File.ReadAllText("fisier.txt");
string json = JsonSerializer.Serialize(obj);
ClasaMea obj = JsonSerializer.Deserialize<ClasaMea>(json);

// WinForms
textBox1.Text           // get/set text
button1.Enabled = false // dezactivează
comboBox1.SelectedItem  // obține selectat
MessageBox.Show("Salut") // popup
form2.ShowDialog()      // deschide formular modal
```

---

## Termeni Esențiali în Engleză

| Termen Englez | Traducere/Explicație în Română |
|---------------|-------------------------------|
| **Value type** | Tip valoare - stocată direct pe stack |
| **Reference type** | Tip referință - pointer către date pe heap |
| **Stack** | Zonă de memorie rapidă, automată, pentru variabile locale |
| **Heap** | Zonă de memorie mare pentru obiecte |
| **Garbage Collector** | Colector de gunoi - eliberează memoria automat |
| **Compile** | Compilare - convertirea codului în executabil |
| **Runtime** | Timpul de execuție al programului |
| **Exception** | Excepție - eroare care poate fi prinsă și tratată |
| **Event** | Eveniment - acțiune declanșată de utilizator sau sistem |
| **Event Handler** | Handler de eveniment - metodă care răspunde la un event |
| **Property** | Proprietate - accesor controlat pentru câmpuri |
| **Field** | Câmp - variabilă de instanță a unei clase |
| **Method** | Metodă - funcție a unei clase |
| **Constructor** | Constructor - metodă specială apelată la crearea obiectului |
| **Inheritance** | Moștenire - o clasă derivă din alta |
| **Polymorphism** | Polimorfism - același cod, comportament diferit |
| **Encapsulation** | Încapsulare - ascunderea implementării interne |
| **Abstraction** | Abstractizare - ascunderea complexității |
| **Interface** | Interfață - contract pe care o clasă trebuie să-l implementeze |
| **Override** | Suprascriere - redefinirea unei metode din clasa părinte |
| **Virtual** | Virtual - metodă care poate fi suprascrisă |
| **Abstract** | Abstract - clasă/metodă incompletă, trebuie implementată |
| **Partial class** | Clasă parțială - definită în mai multe fișiere |
| **Namespace** | Spațiu de nume - organizare logică a codului |
| **Assembly** | Ansamblu - unitate de compilare (.exe sau .dll) |
| **Debug** | Depanare - găsirea și repararea erorilor |
| **Breakpoint** | Punct de întrerupere - oprește execuția pentru debugging |
| **Binding** | Legare - conectarea datelor la UI |
| **Serialization** | Serializare - convertirea obiectului în format stocat |
| **Deserialization** | Deserializare - convertirea formatului stocat în obiect |
| **Modal** | Modal - fereastră care blochează interacțiunea cu restul |
| **Non-modal** | Non-modal - fereastră care permite interacțiunea cu restul |
| **Control** | Control - element de interfață (Button, TextBox, etc.) |
| **Container** | Container - control care conține alte controale |
| **Form** | Formular - fereastră principală a aplicației |
| **Loop** | Buclă - repetarea unui bloc de cod |
| **Array** | Matrice - colecție de dimensiune fixă |
| **List** | Listă - colecție de dimensiune dinamică |
| **Dictionary** | Dicționar - colecție de perechi cheie-valoare |
| **LINQ** | Language Integrated Query - sintaxă pentru interogări |
| **Lambda** | Lambda - funcție anonimă inline |
| **Delegate** | Delegat - referință la o metodă |
| **Null** | Null - absența unei valori |
| **Nullable** | Nullable - tip care poate fi null |
| **Cast** | Conversie explicită de tip |
| **Parse** | Parsare - convertirea string în alt tip |
| **Try/Catch** | Bloc de gestionare a excepțiilor |
| **Finally** | Finally - bloc care rulează întotdeauna |
| **Throw** | Aruncă - generează o excepție |
| **Static** | Static - aparține clasei, nu instanței |
| **Instance** | Instanță - obiect creat din clasă |
| **Access modifier** | Modificator de acces (public, private, etc.) |
| **Return type** | Tip de returnare al unei metode |
| **Parameter** | Parametru - variabilă de intrare pentru metodă |
| **Argument** | Argument - valoarea efectivă transmisă |
| **Scope** | Domeniu de vizibilitate al variabilei |
| **Indexer** | Indexator - permite accesul prin [] |

---

**Succes la concurs!** 🏆
