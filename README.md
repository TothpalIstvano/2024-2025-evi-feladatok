## <p align="center">2024-2025 évi feladatok</p>
###  **SQL**:
- #### <h3>SQL nugget needed:</h3>
     - >MySqlConnector <span>&#8594;</span> ez egy NuGet csomag <br>
        <img src="https://api.nuget.org/v3-flatcontainer/mysqlconnector/2.4.0/icon" alt="MySqlConnector ikon" style="width: 100px; display: block; margin-top: 20px;">
     - >Elérési útvonal <span>&#8594;</span> <ins> Tools <span>&#8594;</span> Nuget package manager <span>&#8594;</span> Manage NuGet packeges for Sulution </ins> <br> <img src="https://www.serkanseker.com/wp-content/uploads/2021/01/Package-Manager-Settings.png" alt="MySqlConnector ikon" style="width: 450px; display: block; margin-top: 20px;">
- #### <h3>Builder:</h3>
    ``` c#
        var builder = new MySqlConnectionStringBuilder
            {
                Server = "127.0.0.1",
                UserID = "root",
                Password = "",
                Database = "Ingatlan"

            };
    ```
- #### <h3>Connection:</h3>
    ``` c#
        static MySqlConnection kapcsolat;
        kapcsolat = new MySqlConnection(builder.ConnectionString);
        kapcsolat.Open();
    ```
- #### <h3>Command:</h3>
    ``` c#
        var command = kapcsolat.CreateCommand();
        command.CommandText = "Select * From sellers Where id in (Select sellerid From realestates) order by name";
    ```
- #### <h3>Command execution:</h3>
    ```c#
        var reader = command.ExecuteReader();
    ```
- #### <h3>Example Function:</h3>
    ``` c#
        private void btnHirdetesek_Click(object sender, EventArgs e)
        {
            var command = kapcsolat.CreateCommand();
            command.CommandText = $"Select * From realestates where sellerid = {activeSellers[listBoxSellers.SelectedIndex].Id}";
            var reader = command.ExecuteReader();
            listBoxCordinates.Items.Clear();
            while (reader.Read())
            {
                string a = "";
                listBoxCordinates.Items.Add($"Hirdetés száma: {reader.GetInt64("id")}");
                listBoxCordinates.Items.Add($"Szobák száma: {reader.GetInt64("rooms")}");
                listBoxCordinates.Items.Add($"Terület: {reader.GetInt64("area")}");
                listBoxCordinates.Items.Add($"Kordináta: {reader.GetString("latlong")}");
                listBoxCordinates.Items.Add("");
            }
            reader.Close();
            lblCount.Text = $"Hirdetések száma: {(listBoxCordinates.Items.Count/5).ToString()}";
        }
    ```
- #### <h3>SQL Parancsok:</h3>

    ### **1. Adatdefiníciós parancsok (DDL - Data Definition Language)**
    | Parancs | Leírás | Példa |
    |---------|--------|-------|
    | `CREATE` | Új adatbázis/tábla/index létrehozása | `CREATE TABLE termekek (id INT, nev VARCHAR(50));` |
    | `ALTER` | Meglévő struktúra módosítása | `ALTER TABLE termekek ADD ar DECIMAL(10,2);` |
    | `DROP` | Objektum törlése | `DROP TABLE termekek;` |
    | `TRUNCATE` | Tábla adatainak teljes törlése | `TRUNCATE TABLE termekek;` |
    | `RENAME` | Átnevezés | `RENAME TABLE regi_nev TO uj_nev;` |

    ### **2. Adatkezelési parancsok (DML - Data Manipulation Language)**
    | Parancs | Leírás | Példa |
    |---------|--------|-------|
    | `SELECT` | Adatok lekérdezése | `SELECT * FROM termekek;` |
    | `INSERT` | Új rekord beszúrása | `INSERT INTO termekek VALUES (1, 'Egér', 25.99);` |
    | `UPDATE` | Meglévő rekord módosítása | `UPDATE termekek SET ar = 29.99 WHERE id = 1;` |
    | `DELETE` | Rekord törlése | `DELETE FROM termekek WHERE id = 1;` |
    | `MERGE` | Beszúrás/frissítés együtt (UPSERT) | `MERGE INTO cel_tabla USING forras...` |

    ### **3. Tranzakciókezelés (TCL - Transaction Control Language)**
    | Parancs | Leírás | Példa |
    |---------|--------|-------|
    | `COMMIT` | Változtatások véglegesítése | `COMMIT;` |
    | `ROLLBACK` | Változtatások visszavonása | `ROLLBACK;` |
    | `SAVEPOINT` | Visszaállítási pont létrehozása | `SAVEPOINT pont1;` |

    ### **4. Jogosultságkezelés (DCL - Data Control Language)**
    | Parancs | Leírás | Példa |
    |---------|--------|-------|
    | `GRANT` | Jogosultság adás | `GRANT SELECT ON termekek TO user1;` |
    | `REVOKE` | Jogosultság visszavonás | `REVOKE SELECT ON termekek FROM user1;` |

    ### **5. Gyakori egyéb parancsok**
    | Parancs | Leírás | Példa |
    |---------|--------|-------|
    | `USE` | Adatbázis kiválasztása | `USE uzlet;` |
    | `DESCRIBE` | Tábla szerkezetének megjelenítése | `DESCRIBE termekek;` |
    | `SHOW` | Meta-információk lekérdezése | `SHOW TABLES;` |
    | `EXPLAIN` | Lekérdezési terv megjelenítése | `EXPLAIN SELECT * FROM termekek;` |
    | `WITH` | Common Table Expression (CTE) | `WITH temp AS (SELECT * FROM termekek) SELECT * FROM temp;` |

    ### **6. Gyakori függvények és operátorok**
    | Kategória | Példák |
    |-----------|--------|
    | **Aggregátumok** | `COUNT()`, `SUM()`, `AVG()`, `MIN()`, `MAX()` |
    | **String függvények** | `CONCAT()`, `SUBSTRING()`, `UPPER()`, `LOWER()` |
    | **Dátum függvények** | `NOW()`, `DATE_FORMAT()`, `DATEDIFF()` |
    | **Feltételesek** | `CASE WHEN`, `COALESCE()`, `NULLIF()` |
    | **JOIN típusok** | `INNER JOIN`, `LEFT JOIN`, `RIGHT JOIN`, `FULL OUTER JOIN` |

    ### **7. Gyakori záradékok (Clauses)**
    | Záradék | Példa |
    |---------|-------|
    | `WHERE` | `SELECT * FROM termekek WHERE ar > 100;` |
    | `GROUP BY` | `SELECT kategoria, COUNT(*) FROM termekek GROUP BY kategoria;` |
    | `HAVING` | `SELECT kategoria FROM termekek GROUP BY kategoria HAVING COUNT(*) > 5;` |
    | `ORDER BY` | `SELECT * FROM termekek ORDER BY ar DESC;` |
    | `LIMIT` | `SELECT * FROM termekek LIMIT 10;` |

    ### **8. Példa komplex lekérdezésre**
    ```sql
    SELECT 
        t.kategoria,
        COUNT(*) AS darab,
        AVG(t.ar) AS atlagar
    FROM 
        termekek t
        INNER JOIN rendelesek r ON t.id = r.termek_id
    WHERE 
        r.datum BETWEEN '2023-01-01' AND '2023-12-31'
    GROUP BY 
        t.kategoria
    HAVING 
        COUNT(*) > 10
    ORDER BY 
        atlagar DESC
    LIMIT 5;
    ```

    ### **9. Adatbázis-specifikus kiegészítések**
    - **MySQL/MariaDB**: `SHOW CREATE TABLE`, `SET FOREIGN_KEY_CHECKS=0`

##

###  **File reader and writer**:
- #### <h3>Reader:</h3>
    ``` c#
        //using System.IO;

        List<string> list = new List<string>();
        StreamReader reader = new StreamReader("filenev.txt", Encoding.UTF8);
        
        while (!reader.EndOfStream)
        {
            list.Add(reader.ReadLine());
        }
        reader.close();
    ```
- #### <h3>Writer:</h3>
    ``` c#
        using System.IO;

        // Data to write
        List<string> lines = new List<string>
        {
            "First line",
            "Second line",
            "Third line"
        };

        // Write to file
        StreamWriter writer = new StreamWriter("output.txt", false, Encoding.UTF8);
        // If false then rewrites the file if true it changes it
        foreach (string line in lines)
        {
            writer.WriteLine(line); // Writes each line with newline
        }
        
        // You can also write without newline:
        writer.Write("This will be on the same line");

        writer.close();
    ```

##

###  **GUI + Console**:
- #### <h3>Checkbox function:</h3>
    ```c#
        static CheckBox[,] checkboxes = new CheckBox[31, 27];
        for (int i = 0; i < 31; i++)
        {
            for (int j = 0; j < 27; j++)
            {
                checkboxes[i, j] = new CheckBox();
                checkboxes[i, j].Location = new Point(130 + i * 20, 101 + j * 22);
                checkboxes[i, j].Size = new Size(20, 20);
                checkboxes[i, j].CheckedChanged += new System.EventHandler(checkBoxes_Checked);
                checkboxes[i, j].Visible = true;
                checkboxes[i, j].Enabled = false;
                this.Controls.Add(checkboxes[i, j]);
            }
        }
    ```
- #### <h3>Button:</h3>
    ```c#
        static Button button_Kilepes = new Button();
        button_Kilepes.Size = new Size(100, 40);
        button_Kilepes.Text = "Kilépés";
        button_Kilepes.Click += new EventHandler(button_Kilepes_Click);
        button_Kilepes.Location = new Point(40, 240);
        this.Controls.Add(button_Kilepes);
    ```
- #### <h3>TableLayoutPanel:</h3>
    ```c#
        using System.Windows.Forms;

        public class FormWithTableLayout : Form
        {
            public FormWithTableLayout()
            {
                // Create and configure the TableLayoutPanel
                TableLayoutPanel tableLayout = new TableLayoutPanel
                {
                    Dock = DockStyle.Fill,
                    ColumnCount = 3,
                    RowCount = 4,
                    CellBorderStyle = TableLayoutPanelCellBorderStyle.Single,
                    BackColor = Color.White
                };

                // Set column widths (percentages)
                tableLayout.ColumnStyles.Add(new ColumnStyle(SizeType.Percent, 30));
                tableLayout.ColumnStyles.Add(new ColumnStyle(SizeType.Percent, 40));
                tableLayout.ColumnStyles.Add(new ColumnStyle(SizeType.Percent, 30));

                // Set row heights
                tableLayout.RowStyles.Add(new RowStyle(SizeType.Absolute, 40));  // Header
                tableLayout.RowStyles.Add(new RowStyle(SizeType.Percent, 25));
                tableLayout.RowStyles.Add(new RowStyle(SizeType.Percent, 25));
                tableLayout.RowStyles.Add(new RowStyle(SizeType.Absolute, 60));  // Footer

                // Add controls to cells
                // Row 0 (Header)
                tableLayout.Controls.Add(new Label { Text = "ID", Font = new Font("Arial", 10, FontStyle.Bold) }, 0, 0);
                tableLayout.Controls.Add(new Label { Text = "Product", Font = new Font("Arial", 10, FontStyle.Bold) }, 1, 0);
                tableLayout.Controls.Add(new Label { Text = "Price", Font = new Font("Arial", 10, FontStyle.Bold) }, 2, 0);

                // Row 1
                tableLayout.Controls.Add(new Label { Text = "1" }, 0, 1);
                tableLayout.Controls.Add(new Label { Text = "Laptop" }, 1, 1);
                tableLayout.Controls.Add(new Label { Text = "$999.99" }, 2, 1);

                // Row 2
                tableLayout.Controls.Add(new Label { Text = "2" }, 0, 2);
                tableLayout.Controls.Add(new Label { Text = "Mouse" }, 1, 2);
                tableLayout.Controls.Add(new Label { Text = "$19.99" }, 2, 2);

                // Row 3 (Footer)
                Button btnClose = new Button { Text = "Close", Dock = DockStyle.Fill };
                btnClose.Click += (sender, e) => this.Close();
                tableLayout.Controls.Add(btnClose, 0, 3);
                tableLayout.SetColumnSpan(btnClose, 3);  // Make button span all columns

                this.Controls.Add(tableLayout);
                this.Text = "TableLayoutPanel Demo";
                this.Size = new Size(400, 300);
            }
        }
        // Usage:
        Application.Run(new FormWithTableLayout());
    ```
- #### <h3>Class:</h3>
    ```c#
        public class Person
        {
            // Public fields (unlike properties, these have no get/set)
            public string FirstName;
            public string LastName;
            public int Age;
            
            // Single property with get/set
            private string _idNumber;
            public string IdNumber
            {
                get => _idNumber;
                set => _idNumber = value ?? throw new ArgumentNullException(nameof(value));
            }
            
            // Constructor
            public Person(string s)
            {
                string[] sorok = s.split(",")
                FirstName = sorok[0];
                LastName = sorok[1];
                Age = int.Parse(sorok[2]);
                IdNumber = int.Parse(sorok[3]); // Uses the property setter
            }
            
            // Public method
            public string GetFullName() => $"{FirstName} {LastName}";
            
            // Public method with validation
            public void CelebrateBirthday()
            {
                Age++;
                Console.WriteLine($"Happy birthday, {FirstName}! Now you're {Age}.");
            }
        }
        //Using class
        var person = new Person("John, Doe, 30, ID-12345");

        // Access public fields directly
        person.Age = 31; 
        Console.WriteLine(person.FirstName); // "John"

        // Access the property
        string id = person.IdNumber; // uses getter
        person.IdNumber = "ID-54321"; // uses setter

        // Call methods
        Console.WriteLine(person.GetFullName()); // "John Doe"
        person.CelebrateBirthday(); // "Happy birthday, John! Now you're 32." 
    ```
#

- #### <h3>Példa kód:</h3>
       ```c#
          /*
          using MySqlConnector;
          using System;
          using System.Collections.Generic;
          using System.ComponentModel;
          using System.Data;
          using System.Drawing;
          using System.IO;
          using System.Linq;
          using System.Text;
          using System.Threading.Tasks;
          using System.Windows.Forms;
          */
          namespace gyak
          {
              public partial class Form1 : Form
              {
                  static MySqlConnection kapcsolat;
                  static TableLayoutPanel tableLayoutPanel1 = new TableLayoutPanel();
                  public Form1()
                  {
                      InitializeComponent();
          
                      var builder = new MySqlConnectionStringBuilder
                      {
                          Server = "127.0.0.1",
                          UserID = "root",
                          Password = "mysql",
                          Database = "Ingatlan"
          
                      };
          
                      kapcsolat = new MySqlConnection(builder.ConnectionString);
                      kapcsolat.Open();
                  }
          
                  private void Form1_Load(object sender, EventArgs e)
                  {
                      var command = kapcsolat.CreateCommand();
                      command.CommandText = "Select COUNT(Nev) From eladok";
          
                      var reader = command.ExecuteReader();
                      if (reader.Read())
                      {
                          tableLayoutPanel1.RowCount = reader.GetInt32(0);
                      }
                      reader.Close();
                      tableLayoutPanel1.ColumnCount = 3;
                      tableLayoutPanel1.Width = 600;
                      tableLayoutPanel1.Height = 60 * tableLayoutPanel1.RowCount;
                      tableLayoutPanel1.CellBorderStyle = TableLayoutPanelCellBorderStyle.Single;
                      tableLayoutPanel1.BackColor = Color.Green;
                      this.Controls.Add(tableLayoutPanel1);
          
                      command.CommandText = "Select Nev, Telefon, Email From eladok";
                      reader = command.ExecuteReader();
                      int i = 0;
                      while (reader.Read())
                      {
                          i++;
                          var nev = new Label();
                          nev.Text = reader.GetString(0);
                          nev.Dock = DockStyle.Fill;
                          nev.TextAlign = ContentAlignment.MiddleCenter;
                          tableLayoutPanel1.Controls.Add(nev, 0, i - 1);
                          var telefon = new Label();
                          telefon.Text = reader.GetString(1);
                          telefon.Dock = DockStyle.Fill;
                          telefon.TextAlign = ContentAlignment.MiddleCenter;
                          tableLayoutPanel1.Controls.Add(telefon, 1, i - 1);
                          var email = new Label();
                          email.Text = reader.GetString(2);
                          email.Dock = DockStyle.Fill;
                          email.TextAlign = ContentAlignment.MiddleCenter;
                          tableLayoutPanel1.Controls.Add(email, 2, i - 1);
                      }
                      for(int j = 0; j < 3; j++)
                      {
                          tableLayoutPanel1.ColumnStyles.Add(new ColumnStyle(SizeType.Percent, 100f / 3));
                      }
                      this.Controls.Add(tableLayoutPanel1);
          
                      reader.Close();
          
                      StreamReader reader2 = new StreamReader("ingatlan.txt", Encoding.UTF8);
                      List<string> list = new List<string>();
                      while (!reader2.EndOfStream)
                      {
                          list.Add(reader2.ReadLine());
                      }
                      string[,] strings = new string[8, list.Count];
                      for (int j = 0; j < list.Count; j++)
                      {
                          var sor = list[j].Split('|');
                          for (int k = 0; k < 8; k++)
                          {
                              strings[k, j] = sor[k];
                          }
                      }
                      DataTable table = new DataTable();
                      table.Columns.Add("Azonosító");
                      table.Columns.Add("Eladó");
                      table.Columns.Add("Település");
                      table.Columns.Add("Irányítószám");
                      table.Columns.Add("Cím");
                      table.Columns.Add("Alapterület");
                      table.Columns.Add("Szobák száma");
                      table.Columns.Add("Ár");
                      for (int j = 0; j < list.Count; j++)
                      {
                          table.Rows.Add(strings[0, j], strings[1, j], strings[2, j], strings[3, j], strings[4, j], strings[5, j], strings[6, j], strings[7, j]);
                      }
                      var dataGridView1 = new DataGridView();
                      dataGridView1.DataSource = table;
                      dataGridView1.Dock = DockStyle.Bottom;
                      dataGridView1.Height = 200;
                      this.Controls.Add(dataGridView1);
                         
                      reader2.Close();
                      kapcsolat.Close();
                  }
              }
          }
       ```
