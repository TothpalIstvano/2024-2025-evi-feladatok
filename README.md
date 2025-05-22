## <p align="center">2024-2025 évi feladatok</p>
###  **SQL**:
- #### <h3>SQL nugget needed:</h3>
     - >MySqlConnector <span>&#8594;</span> ez egy NuGet csomag <br>
        <img src="https://api.nuget.org/v3-flatcontainer/mysqlconnector/2.4.0/icon" alt="MySqlConnector ikon" style="width: 100px; display: block; margin-top: 20px;">
     - >Elérési útvonal <span>&#8594;</span> Tools <span>&#8594;</span> Nuget package manager         <img src="https://www.serkanseker.com/wp-content/uploads/2021/01/Package-Manager-Settings.png" alt="MySqlConnector ikon" style="width: 400px; display: block; margin-top: 20px;">
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
##

###  **File reader and writer**:
- #### <h3>Reader:</h3>
    ``` c#

    ```