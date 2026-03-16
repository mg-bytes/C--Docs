
---
!!!!
Am scris majoritateaz variabilelor cu var. var in C# poate lua orice forma, e un mod mai simlpu de a scrie.
EX:

int a = 0; // Aici stii ca a e un int.
var b = 0; // Aici programul deduce singur ce ar fi b in functie de ce e dupa egal. In C++ exista asa ceva si se numeste auto in loc de var.
Merge cu absolut orice, clase create de tine, clasele deja prestabilite (int, float, double, char, string, List orice)

var text = "Buna Igwana" <=> string text = "Buna Igwana"

Iti zic asta pentru ca la olimpiada e mai usor cam sa pui var peste tot si mai rapid. Isi da seama programul in functie de ce e dupa egal.




### 1. Navigare între formulare
!!! Close inchide complet formularul, hide il baga doar in bara cum ar veni.
```csharp
this.Hide();                          // ascunde formularul curent
new FormUrmator().Show();             // deschide nou

this.Close();                         // închide curent
new FormUrmator(variabila).Show();    // pasezi date si il descide
```

---

### 2. AppData static
```csharp
static class AppData
{
    public static List<Insula> Insule = new List<Insula>();
    public static string Resurse = Path.Combine(
        Application.StartupPath, "NumeFolderResurse");
}
```
Accesat de oriunde direct: `AppData.Insule`

---

### 3. Citit fișier
```csharp
foreach (string linie in File.ReadAllLines(
    Path.Combine(AppData.Resurse, "fisier.txt")))
{
    string[] p = linie.Split(';'); // sau '#' sau orice separator, te gandesti mereu la despartirea a doi gujbeti
    // p[0], p[1], p[2]...
}
```

---

### 4. Clase simple
```csharp
class Insula
{
    public int Id { get; set; }
    public string Nume { get; set; }
    public int X { get; set; }
    public int Y { get; set; }
    public bool AreBogatii { get; set; }
    public bool AreVirusuri { get; set; }

    CE LIPSESTE AICI?? :) 

}
```

---

### 5. List — operații esențiale
```csharp
lista.Add(obiect);
lista.Remove(obiect);
lista.Count;

// Căutare
var x = lista.FirstOrDefault(i => i.Id == 5);
bool exista = lista.Any(i => i.Nume == "Ana");

// Filtrare + sortare + primele N
var top3 = lista
    .Where(i => i.TipJoc == 0) // Adica toate elementele din lista unde tipul de joc este = 0.
    .OrderByDescending(i => i.Punctaj) // Descrescator in functie de Punctaj. Crescator era OrderByAscending(i => i.Punctaj)
    .Take(3) // Primele 3 elemente din lista
    .ToList(); // Mereu la final adaugi ToList(); ca sa iti creeze o lista pe care o mai poti manipula ulterior daca ai nevoie.
```

---

### 6. Validare input
```csharp
if (int.TryParse(textBox1.Text, out int n) && n >= 30 && n <= 200)
{
    // valid
}
else
{
    MessageBox.Show("Eroare!");
    textBox1.Clear();
}
```

---

### 7. PasswordChar
```csharp
textBoxParola.PasswordChar = '*';
```

---

### 8. PictureBox cu imagine din fișier
```csharp
pictureBox1.Image = Image.FromFile(
    Path.Combine(AppData.Resurse, "imagine.png"));
pictureBox1.SizeMode = PictureBoxSizeMode.StretchImage;
```

---

### 9. Controale dinamice
```csharp
PictureBox pb = new PictureBox();
pb.Image = Image.FromFile(cale);
pb.Size = new Size(50, 50);
pb.Location = new Point(x - 25, y - 25);
pb.SizeMode = PictureBoxSizeMode.StretchImage;
pb.Click += (s, e) => MetodaClick();
panel1.Controls.Add(pb);
```

---

### 10. Timer
```csharp
Timer timer = new Timer();
timer.Interval = 1000;
timer.Tick += (s, e) => {
    // logică repetată
    panel1.Invalidate(); // redesenează dacă e cazul
};
timer.Start();
timer.Stop();
```

---

### 11. Desenat pe Panel
```csharp
// În Load:
panel1.Paint += Panel1_Paint;

void Panel1_Paint(object sender, PaintEventArgs e)
{
    Graphics g = e.Graphics;
    g.FillEllipse(Brushes.White, x, y, 20, 20);
    g.FillRectangle(Brushes.Green, x, y, 20, 20);
    g.DrawLine(new Pen(Color.Green, 2), p1, p2);
    g.DrawString("text", Font, Brushes.White, x, y);
}

// Când vrei să redesenezi:
panel1.Invalidate();
```

---

### 12. Random
```csharp
Random rnd = new Random();
int nr = rnd.Next(10, 101);       // 10 până la 100 inclusiv
int i = rnd.Next(lista.Count);    // index aleator din listă
```

---

### 13. KeyPreview + taste
```csharp
// În constructor:
this.KeyPreview = true;

private void Form_KeyDown(object sender, KeyEventArgs e)
{
    if (e.KeyCode == Keys.A) { }
    if (e.KeyCode == Keys.W) { }
    if (e.Control && e.KeyCode == Keys.S) { }
}
```

---

### 14. SaveFileDialog + salvat imagine
```csharp
SaveFileDialog sfd = new SaveFileDialog();
sfd.FileName = "TraseuExplorator.jpg";
sfd.Filter = "JPEG|*.jpg";

if (sfd.ShowDialog() == DialogResult.OK)
{
    Bitmap bmp = new Bitmap(panel1.Width, panel1.Height);
    panel1.DrawToBitmap(bmp, panel1.ClientRectangle);
    bmp.Save(sfd.FileName, System.Drawing.Imaging.ImageFormat.Jpeg);
}
```

---

### 15. Scris în fișier
```csharp
// Suprascrie tot:
File.WriteAllLines(cale, lista.Select(
    r => $"{r.Id};{r.TipJoc};{r.Email};{r.Punctaj}"));

// Adaugă o linie:
File.AppendAllText(cale, $"{r.Id};{r.TipJoc}\n");
```

---

### 16. RadioButton + Button dezactivat
```csharp
btnInregistreaza.Enabled = false;

private void rb_CheckedChanged(object sender, EventArgs e)
{
    btnInregistreaza.Enabled = true;
}
```

---

### 17. Animare liniară cu Timer
```csharp
Point pozCurenta, destinatie;
int pasi = 30, pasulCurent = 0;
double pasX, pasY;

void StartAnimatie(Point dest)
{
    destinatie = dest;
    pasX = (dest.X - pozCurenta.X) / (double)pasi;
    pasY = (dest.Y - pozCurenta.Y) / (double)pasi;
    pasulCurent = 0;
    timer.Start();
}

void Timer_Tick(object sender, EventArgs e)
{
    pasulCurent++;
    pictureBoxBarca.Left += (int)pasX;
    pictureBoxBarca.Top += (int)pasY;

    if (pasulCurent >= pasi)
    {
        timer.Stop();
        pictureBoxBarca.Location = destinatie;
        ProcesesazaAcostare();
    }
}
```

---

# C# Windows Forms - Cheat Sheet pentru Ioana

---

## 1. Variabile și Tipuri de Date

```csharp
int varsta = 17;                 // numere întregi
double pret = 19.99;             // numere cu zecimale
bool esteElevaă = true;          // adevărat sau fals
char nota = 'A';                 // un singur caracter
string nume = "Ioana";           // text
int[] note = {10, 9, 8};         // array (listă fixă)
```

### Conversie din string în număr

```csharp
string input = textBox1.Text;

// Varianta sigură (recomandat!)
if (int.TryParse(input, out int rezultat))
{
    // rezultat conține numărul
}
else
{
    MessageBox.Show("Nu e un număr valid!");
}

// Pentru double
if (double.TryParse(input, out double valoare))
{
    // valoare conține numărul
}
```

---

## 2. Operatori

| Tip | Operatori | Exemplu |
|-----|-----------|---------|
| Aritmetici | `+  -  *  /  %` | `5 + 3`, `10 % 3` (rest = 1) |
| Comparație | `==  !=  <  >  <=  >=` | `x == 5`, `varsta >= 18` |
| Logici | `&&  \|\|  !` | `&&` = ȘI, `\|\|` = SAU, `!` = NU |

```csharp
// Exemple
if (varsta >= 18 && arePermis)  // ambele trebuie adevărate
if (esteWeekend || esteVacanta) // cel puțin una adevărată
if (!esteOcupat)                // inversează (dacă NU e ocupat)
```

---

## 3. Decizii (if/else și switch)

### If / Else

```csharp
int scor = 85;

if (scor >= 90)
{
    MessageBox.Show("Nota A");
}
else if (scor >= 80)
{
    MessageBox.Show("Nota B");
}
else
{
    MessageBox.Show("Mai încearcă");
}
```

### Switch

```csharp
string zi = comboBox1.SelectedItem?.ToString();

switch (zi)
{
    case "Luni":
    case "Marți":
        MessageBox.Show("Început de săptămână");
        break;
    case "Vineri":
        MessageBox.Show("Weekend aproape!");
        break;
    default:
        MessageBox.Show("Zi obișnuită");
        break;
}
```

---

## 4. Bucle (Loops)

### For - când știi de câte ori

```csharp
for (int i = 0; i < 5; i++)
{
    listBox1.Items.Add("Element " + i);
}
```

### Foreach - pentru liste și colecții

```csharp
List<string> eleve = new List<string> {"Ioana", "Maria", "Ana"};
foreach (string nume in eleve)
{
    listBox1.Items.Add(nume);
}
```

### While - cât timp condiția e adevărată

```csharp
int contor = 0;
while (contor < 10)
{
    contor++;
}
```

**Cuvinte cheie utile:**
- `break;` - ieși din buclă imediat
- `continue;` - sari la următoarea iterație

---

## 5. Colecții

### List - listă dinamică (cea mai folosită)

```csharp
List<string> eleve = new List<string>();

eleve.Add("Ioana");              // adaugă la sfârșit
eleve.Add("Maria");
eleve.Insert(0, "Ana");          // inserează la poziția 0
eleve.Remove("Maria");           // șterge "Maria"
eleve.RemoveAt(0);               // șterge de la poziția 0
eleve.Clear();                   // șterge tot

int cate = eleve.Count;          // câte elemente are
bool exista = eleve.Contains("Ioana");  // verifică dacă există

// Inițializare directă
List<int> numere = new List<int> {1, 2, 3, 4, 5};
```

### Dictionary - perechi cheie-valoare

```csharp
Dictionary<string, int> note = new Dictionary<string, int>();

note["Ioana"] = 10;
note["Maria"] = 9;
note.Add("Ana", 8);

int notaIoana = note["Ioana"];              // 10
bool exista = note.ContainsKey("Ioana");    // true
note.Remove("Maria");

// Parcurgere
foreach (var pereche in note)
{
    MessageBox.Show(pereche.Key + ": " + pereche.Value);
}
```

---

## 6. Metode (Funcții)

```csharp
// Metodă fără return
void AfiseazaSalut(string nume)
{
    MessageBox.Show("Salut, " + nume + "!");
}

// Metodă cu return
int Aduna(int a, int b)
{
    return a + b;
}

// Apelare
AfiseazaSalut("Ioana");
int suma = Aduna(5, 3);  // suma = 8
```

---

## 7. Clase și Obiecte

```csharp
public class Eleva
{
    // Proprietăți
    public string Nume { get; set; }
    public int Varsta { get; set; }
    public double Medie { get; set; }
    
    // Constructor
    public Eleva(string nume, int varsta)
    {
        Nume = nume;
        Varsta = varsta;
        Medie = 0;
    }
    
    // Metodă
    public void CalculeazaMedie(int[] note)
    {
        double suma = 0;
        foreach (int nota in note)
        {
            suma += nota;
        }
        Medie = suma / note.Length;
    }
    
    // Pentru afișare în ListBox/ComboBox
    public override string ToString()
    {
        return Nume + " - Media: " + Medie.ToString("F2");
    }
}

// Utilizare
Eleva ioana = new Eleva("Ioana", 17);
ioana.CalculeazaMedie(new int[] {10, 9, 10});
listBox1.Items.Add(ioana);
```

---

## 8. Controale Windows Forms - Referință Rapidă

| Control | Ce face | Proprietăți importante |
|---------|---------|------------------------|
| Label | Afișează text | `Text` |
| TextBox | Input text | `Text`, `Multiline`, `ReadOnly` |
| Button | Buton clickabil | `Text`, `Enabled` |
| ComboBox | Dropdown | `Items`, `SelectedItem`, `SelectedIndex` |
| ListBox | Listă selectabilă | `Items`, `SelectedItem`, `SelectedIndex` |
| CheckBox | Bifă da/nu | `Checked`, `Text` |
| RadioButton | Una din opțiuni | `Checked`, `Text` |
| DataGridView | Tabel | `DataSource` |
| DateTimePicker | Selector dată | `Value` |
| NumericUpDown | Input numeric | `Value`, `Minimum`, `Maximum` |
| Timer | Rulează la interval | `Interval`, `Enabled` |

### Citirea și setarea valorilor

```csharp
// CITIRE
string text = textBox1.Text;
string selectat = comboBox1.SelectedItem?.ToString();
int index = comboBox1.SelectedIndex;
bool bifat = checkBox1.Checked;
DateTime data = dateTimePicker1.Value;
decimal numar = numericUpDown1.Value;

// SETARE
label1.Text = "Salut Ioana!";
textBox1.Text = "";
comboBox1.SelectedIndex = 0;
checkBox1.Checked = true;
button1.Enabled = false;  // dezactivează butonul

// LISTE
comboBox1.Items.Add("Opțiune nouă");
comboBox1.Items.Clear();
listBox1.Items.AddRange(new string[] {"A", "B", "C"});
```

---

## 9. Evenimente (Events)

```csharp
// Click pe buton
private void button1_Click(object sender, EventArgs e)
{
    MessageBox.Show("Ai dat click!");
}

// Text schimbat
private void textBox1_TextChanged(object sender, EventArgs e)
{
    label1.Text = textBox1.Text.Length + " caractere";
}

// Formular încărcat (pune aici inițializările)
private void Form1_Load(object sender, EventArgs e)
{
    IncarcaDate();
}

// Selecție schimbată în ListBox
private void listBox1_SelectedIndexChanged(object sender, EventArgs e)
{
    if (listBox1.SelectedItem != null)
    {
        string selectat = listBox1.SelectedItem.ToString();
    }
}

// Timer tick
private void timer1_Tick(object sender, EventArgs e)
{
    label1.Text = DateTime.Now.ToString("HH:mm:ss");
}
```

---

## 10. Lucrul cu Mai Multe Formulare

```csharp
// Deschide alt formular
Form2 form2 = new Form2();
form2.ShowDialog();  // modal - blochează Form1 până se închide
// sau
form2.Show();        // non-modal - ambele active

// ═══════════════════════════════════════════
// TRIMITE DATE către Form2
// ═══════════════════════════════════════════

// În Form2, adaugă o proprietate:
public partial class Form2 : Form
{
    public string DatePrimite { get; set; }
    
    private void Form2_Load(object sender, EventArgs e)
    {
        label1.Text = DatePrimite;
    }
}

// În Form1:
Form2 form2 = new Form2();
form2.DatePrimite = "Salut de la Form1!";
form2.ShowDialog();

// ═══════════════════════════════════════════
// PRIMEȘTE DATE înapoi de la Form2
// ═══════════════════════════════════════════

// În Form2:
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
    MessageBox.Show("Am primit: " + rezultat);
}
```

---

## 11. MessageBox

```csharp
// Simplu
MessageBox.Show("Salvat!");

// Cu titlu
MessageBox.Show("Date salvate cu succes.", "Succes");

// Cu butoane și întrebare
DialogResult rezultat = MessageBox.Show(
    "Sigur vrei să ștergi?", 
    "Confirmare",
    MessageBoxButtons.YesNo,
    MessageBoxIcon.Question);

if (rezultat == DialogResult.Yes)
{
    // Șterge
}
```

---

## 12. DataGridView (Tabele)

```csharp
// Legare la o listă
List<Eleva> eleve = new List<Eleva>();
eleve.Add(new Eleva("Ioana", 17) { Medie = 9.5 });
eleve.Add(new Eleva("Maria", 16) { Medie = 8.7 });

// IMPORTANT: resetează înainte de a seta din nou!
dataGridView1.DataSource = null;
dataGridView1.DataSource = eleve;

// Citirea rândului selectat
private void dataGridView1_CellClick(object sender, DataGridViewCellEventArgs e)
{
    if (e.RowIndex >= 0)
    {
        DataGridViewRow rand = dataGridView1.Rows[e.RowIndex];
        string nume = rand.Cells["Nume"].Value?.ToString();
        textBox1.Text = nume;
    }
}

// Sau cu binding direct la obiect:
if (dataGridView1.CurrentRow != null)
{
    Eleva selectata = (Eleva)dataGridView1.CurrentRow.DataBoundItem;
    textBox1.Text = selectata.Nume;
}
```

---

## 13. Salvare și Încărcare Date

### Fișiere Text Simple

```csharp
using System.IO;

// Scrie
File.WriteAllText("date.txt", "Salut Ioana!");

// Citește
string continut = File.ReadAllText("date.txt");

// Scrie mai multe linii
string[] linii = {"Linia 1", "Linia 2", "Linia 3"};
File.WriteAllLines("date.txt", linii);

// Citește linii
string[] liniiCitite = File.ReadAllLines("date.txt");
```

### JSON (Recomandat pentru obiecte)

```csharp
using System.Text.Json;

// SALVARE
List<Eleva> eleve = GetEleve();
string json = JsonSerializer.Serialize(eleve);
File.WriteAllText("eleve.json", json);

// ÎNCĂRCARE
string json = File.ReadAllText("eleve.json");
List<Eleva> eleve = JsonSerializer.Deserialize<List<Eleva>>(json);

// Cu formatare frumoasă (opțional, pentru debugging)
var optiuni = new JsonSerializerOptions { WriteIndented = true };
string json = JsonSerializer.Serialize(eleve, optiuni);
```

### Dialoguri pentru fișiere

```csharp
// Dialog DESCHIDERE
OpenFileDialog dialog = new OpenFileDialog();
dialog.Filter = "Fișiere JSON (*.json)|*.json|Toate (*.*)|*.*";
if (dialog.ShowDialog() == DialogResult.OK)
{
    string continut = File.ReadAllText(dialog.FileName);
}

// Dialog SALVARE
SaveFileDialog dialog = new SaveFileDialog();
dialog.Filter = "Fișiere JSON (*.json)|*.json";
if (dialog.ShowDialog() == DialogResult.OK)
{
    File.WriteAllText(dialog.FileName, json);
}
```

---

## 14. Gestionarea Erorilor

```csharp
try
{
    int numar = int.Parse(textBox1.Text);
    int rezultat = 100 / numar;
}
catch (FormatException)
{
    MessageBox.Show("Introdu un număr valid!");
}
catch (DivideByZeroException)
{
    MessageBox.Show("Nu poți împărți la zero!");
}
catch (Exception ex)
{
    MessageBox.Show("Eroare: " + ex.Message);
}
```

### Validare Input (Pattern util)

```csharp
private bool ValideazaFormular()
{
    // Verifică gol
    if (string.IsNullOrWhiteSpace(txtNume.Text))
    {
        MessageBox.Show("Numele este obligatoriu!");
        txtNume.Focus();
        return false;
    }
    
    // Verifică număr
    if (!int.TryParse(txtVarsta.Text, out int varsta))
    {
        MessageBox.Show("Vârsta trebuie să fie un număr!");
        txtVarsta.Focus();
        return false;
    }
    
    // Verifică range
    if (varsta < 0 || varsta > 120)
    {
        MessageBox.Show("Vârsta invalidă!");
        return false;
    }
    
    // Verifică selecție
    if (comboBox1.SelectedIndex < 0)
    {
        MessageBox.Show("Selectează o opțiune!");
        return false;
    }
    
    return true;
}

// Utilizare
private void btnSalveaza_Click(object sender, EventArgs e)
{
    if (!ValideazaFormular()) return;
    
    // Continuă cu salvarea...
}
```

---

## 15. LINQ - Filtrare și Sortare

```csharp
using System.Linq;

List<Eleva> eleve = GetEleve();

// Filtrare
var premiate = eleve.Where(e => e.Medie >= 9.5).ToList();

// Sortare
var sortate = eleve.OrderBy(e => e.Nume).ToList();
var sortateDesc = eleve.OrderByDescending(e => e.Medie).ToList();

// Găsește una
var ioana = eleve.FirstOrDefault(e => e.Nume == "Ioana");

// Verifică dacă există
bool arePremiate = eleve.Any(e => e.Medie >= 9.5);

// Numără
int catePremiate = eleve.Count(e => e.Medie >= 9.5);

// Combinat
var rezultat = eleve
    .Where(e => e.Varsta >= 16)
    .OrderBy(e => e.Nume)
    .ToList();
```

---

## 16. Șablon Complet - Aplicație CRUD

```csharp
public partial class MainForm : Form
{
    private List<Eleva> eleve = new List<Eleva>();
    private string fisierDate = "eleve.json";
    
    public MainForm()
    {
        InitializeComponent();
    }
    
    // ═══════════════════════════════════════════
    // LOAD - La pornirea aplicației
    // ═══════════════════════════════════════════
    private void MainForm_Load(object sender, EventArgs e)
    {
        IncarcaDate();
        RefreshGrid();
    }
    
    // ═══════════════════════════════════════════
    // CREATE - Adăugare
    // ═══════════════════════════════════════════
    private void btnAdauga_Click(object sender, EventArgs e)
    {
        if (!ValideazaInput()) return;
        
        var eleva = new Eleva(txtNume.Text, int.Parse(txtVarsta.Text));
        eleva.Medie = double.Parse(txtMedie.Text);
        
        eleve.Add(eleva);
        SalveazaDate();
        RefreshGrid();
        CurataFormular();
    }
    
    // ═══════════════════════════════════════════
    // READ - Afișare
    // ═══════════════════════════════════════════
    private void RefreshGrid()
    {
        dataGridView1.DataSource = null;
        dataGridView1.DataSource = eleve;
    }
    
    // ═══════════════════════════════════════════
    // UPDATE - Actualizare
    // ═══════════════════════════════════════════
    private void btnActualizeaza_Click(object sender, EventArgs e)
    {
        if (dataGridView1.CurrentRow == null)
        {
            MessageBox.Show("Selectează o elevă întâi!");
            return;
        }
        
        if (!ValideazaInput()) return;
        
        int index = dataGridView1.CurrentRow.Index;
        eleve[index].Nume = txtNume.Text;
        eleve[index].Varsta = int.Parse(txtVarsta.Text);
        eleve[index].Medie = double.Parse(txtMedie.Text);
        
        SalveazaDate();
        RefreshGrid();
    }
    
    // ═══════════════════════════════════════════
    // DELETE - Ștergere
    // ═══════════════════════════════════════════
    private void btnSterge_Click(object sender, EventArgs e)
    {
        if (dataGridView1.CurrentRow == null) return;
        
        var rezultat = MessageBox.Show(
            "Sigur vrei să ștergi?", 
            "Confirmare",
            MessageBoxButtons.YesNo, 
            MessageBoxIcon.Warning);
        
        if (rezultat == DialogResult.Yes)
        {
            int index = dataGridView1.CurrentRow.Index;
            eleve.RemoveAt(index);
            SalveazaDate();
            RefreshGrid();
            CurataFormular();
        }
    }
    
    // ═══════════════════════════════════════════
    // SEARCH - Căutare
    // ═══════════════════════════════════════════
    private void txtCautare_TextChanged(object sender, EventArgs e)
    {
        string cautare = txtCautare.Text.ToLower();
        var filtrate = eleve
            .Where(e => e.Nume.ToLower().Contains(cautare))
            .ToList();
        
        dataGridView1.DataSource = null;
        dataGridView1.DataSource = filtrate;
    }
    
    // ═══════════════════════════════════════════
    // SELECȚIE - Când selectezi în grid
    // ═══════════════════════════════════════════
    private void dataGridView1_SelectionChanged(object sender, EventArgs e)
    {
        if (dataGridView1.CurrentRow == null) return;
        
        var eleva = (Eleva)dataGridView1.CurrentRow.DataBoundItem;
        if (eleva != null)
        {
            txtNume.Text = eleva.Nume;
            txtVarsta.Text = eleva.Varsta.ToString();
            txtMedie.Text = eleva.Medie.ToString("F2");
        }
    }
    
    // ═══════════════════════════════════════════
    // HELPER METHODS
    // ═══════════════════════════════════════════
    private void IncarcaDate()
    {
        if (File.Exists(fisierDate))
        {
            string json = File.ReadAllText(fisierDate);
            eleve = JsonSerializer.Deserialize<List<Eleva>>(json) 
                    ?? new List<Eleva>();
        }
    }
    
    private void SalveazaDate()
    {
        string json = JsonSerializer.Serialize(eleve);
        File.WriteAllText(fisierDate, json);
    }
    
    private void CurataFormular()
    {
        txtNume.Clear();
        txtVarsta.Clear();
        txtMedie.Clear();
        txtNume.Focus();
    }
    
    private bool ValideazaInput()
    {
        if (string.IsNullOrWhiteSpace(txtNume.Text))
        {
            MessageBox.Show("Numele este obligatoriu!");
            return false;
        }
        if (!int.TryParse(txtVarsta.Text, out _))
        {
            MessageBox.Show("Vârsta trebuie să fie un număr!");
            return false;
        }
        if (!double.TryParse(txtMedie.Text, out _))
        {
            MessageBox.Show("Media trebuie să fie un număr!");
            return false;
        }
        return true;
    }
}
```

---

## 17. Greșeli Comune de Evitat

```csharp
// ❌ GREȘIT: Nu verifici pentru null
string nume = listBox1.SelectedItem.ToString();  // CRASH dacă nimic selectat!

// ✅ CORECT: Verifică întâi
if (listBox1.SelectedItem != null)
{
    string nume = listBox1.SelectedItem.ToString();
}

// ═══════════════════════════════════════════

// ❌ GREȘIT: Parse direct pe input
int varsta = int.Parse(textBox1.Text);  // CRASH pe input invalid!

// ✅ CORECT: Folosește TryParse
if (int.TryParse(textBox1.Text, out int varsta))
{
    // folosește varsta
}

// ═══════════════════════════════════════════

// ❌ GREȘIT: Uiți să reîmprospătezi grid-ul
eleve.Add(elevaNoua);
// Grid-ul arată datele vechi!

// ✅ CORECT: Refresh după modificări
eleve.Add(elevaNoua);
dataGridView1.DataSource = null;
dataGridView1.DataSource = eleve;

// ═══════════════════════════════════════════

// ❌ GREȘIT: Nu salvezi datele
eleve.Add(elevaNoua);
// Pierdut când închizi aplicația!

// ✅ CORECT: Salvează după fiecare modificare
eleve.Add(elevaNoua);
SalveazaDate();
RefreshGrid();
```

---

## 18. Checklist pentru Concurs

### RULEZI NON-STOP dupa fiecare implementare. Daca ai facut deja o parte din cerinte. Salvezi proiectul intr-o arhiva (RAR,ZIP) inainte sa te apuci de celelalte cerinte. Ca sa ai ce sa arati in caz de strici ceva.

### Înainte să scrii cod:
- [ ] Citește cerințele de 2 ori
- [ ] Stabilește ce clase îți trebuie
- [ ] Schițează UI-ul pe hârtie
- [ ] Decide formatul fișierului (JSON = cel mai simplu)

### UI:
- [ ] Toate butoanele au text clar
- [ ] Câmpurile au etichete (labels)

### Dacă te blochezi:
1. Compilează? Repară erorile!
2. Pune `MessageBox.Show("Am ajuns aici")` pentru a vedea unde ajunge codul. Sau cum te-am invatat cu breakpoints. Dar daca te incurci in ele sau devin prea greu de ajuns la ele, faci cu MessageBox.
3. Verifică valorile cu `MessageBox.Show(variabila.ToString())`

---

## 19. Sintaxă Rapidă - Card de Referință

```csharp
// VARIABILE
int x = 5;
string s = "text";
bool b = true;
List<int> lista = new List<int>();
Dictionary<string, int> dict = new Dictionary<string, int>();

// DECIZII
if (x > 0) { } else if (x < 0) { } else { }
switch (x) { case 1: break; default: break; }

// BUCLE
for (int i = 0; i < 10; i++) { }
foreach (var item in lista) { }
while (conditie) { }

// METODE
void FaCeva() { }
int Aduna(int a, int b) { return a + b; }

// CLASE
public class Eleva
{
    public string Nume { get; set; }
    public Eleva(string nume) { Nume = nume; }
}

// FILE I/O
File.WriteAllText("fisier.txt", continut);
string continut = File.ReadAllText("fisier.txt");
string json = JsonSerializer.Serialize(obj);
Eleva e = JsonSerializer.Deserialize<Eleva>(json);

// WINFORMS
textBox1.Text              // get/set text
button1.Enabled = false    // dezactivează
comboBox1.SelectedItem     // elementul selectat
MessageBox.Show("text")    // popup
form2.ShowDialog()         // deschide formular modal
```

---

**Daca inveti toti ce e aici poti sa te contrazici si cu Teo. Cam e toata materia din primul semetru de facultate. ** 🏆
