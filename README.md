using System;
using System.Data;
using System.Data.SqlClient;

class Program
{
    static string connectionString = "YourConnectionStringHere"; // Replace with your actual connection string

    static void Main()
    {
        // Task 1: Create Database
        CreateDatabase();

        // Task 2: Create Table Employee with Primary Key Constraint
        CreateTable();

        // Task 3: Print All Information
        PrintAllInformation();

        // Task 4: CRUD Operations
        InsertEmployee("John Doe", 25, "john.doe@example.com");
        PrintAllInformation();

        UpdateEmployee(1, "Updated John Doe", 30, "updated.john.doe@example.com");
        PrintAllInformation();

        DeleteEmployee(1);
        PrintAllInformation();

        // Task 5: Update Information using ID with Validation
        int employeeIdToUpdate = 2; // Replace with the actual ID you want to update
        UpdateEmployeeWithValidation(employeeIdToUpdate);

        // Task 6: Display Information in Data Grid View (Assuming a Windows Forms Application)
        // You need to create a Windows Forms Application for this task.
    }

    static void CreateDatabase()
    {
        using (SqlConnection connection = new SqlConnection(connectionString))
        {
            connection.Open();

            string createDatabaseQuery = "CREATE DATABASE EmployeeDatabase";
            using (SqlCommand command = new SqlCommand(createDatabaseQuery, connection))
            {
                command.ExecuteNonQuery();
            }
        }
    }

    static void CreateTable()
    {
        using (SqlConnection connection = new SqlConnection(connectionString))
        {
            connection.Open();

            string createTableQuery = "USE EmployeeDatabase; " +
                                      "CREATE TABLE Employee ( " +
                                      "Id INT PRIMARY KEY IDENTITY(1,1), " +
                                      "Name NVARCHAR(50), " +
                                      "Age INT, " +
                                      "Email NVARCHAR(50))";
            using (SqlCommand command = new SqlCommand(createTableQuery, connection))
            {
                command.ExecuteNonQuery();
            }
        }
    }

    static void PrintAllInformation()
    {
        using (SqlConnection connection = new SqlConnection(connectionString))
        {
            connection.Open();

            string selectQuery = "USE EmployeeDatabase; SELECT * FROM Employee";
            using (SqlCommand command = new SqlCommand(selectQuery, connection))
            {
                using (SqlDataReader reader = command.ExecuteReader())
                {
                    Console.WriteLine("Employee Information:");
                    Console.WriteLine("ID\tName\t\tAge\tEmail");
                    while (reader.Read())
                    {
                        Console.WriteLine($"{reader["Id"]}\t{reader["Name"]}\t{reader["Age"]}\t{reader["Email"]}");
                    }
                    Console.WriteLine();
                }
            }
        }
    }

    static void InsertEmployee(string name, int age, string email)
    {
        using (SqlConnection connection = new SqlConnection(connectionString))
        {
            connection.Open();

            string insertQuery = "USE EmployeeDatabase; " +
                                 "INSERT INTO Employee (Name, Age, Email) VALUES (@Name, @Age, @Email)";
            using (SqlCommand command = new SqlCommand(insertQuery, connection))
            {
                command.Parameters.AddWithValue("@Name", name);
                command.Parameters.AddWithValue("@Age", age);
                command.Parameters.AddWithValue("@Email", email);

                command.ExecuteNonQuery();
            }
        }
    }

    static void UpdateEmployee(int id, string name, int age, string email)
    {
        using (SqlConnection connection = new SqlConnection(connectionString))
        {
            connection.Open();

            string updateQuery = "USE EmployeeDatabase; " +
                                 "UPDATE Employee SET Name = @Name, Age = @Age, Email = @Email WHERE Id = @Id";
            using (SqlCommand command = new SqlCommand(updateQuery, connection))
            {
                command.Parameters.AddWithValue("@Id", id);
                command.Parameters.AddWithValue("@Name", name);
                command.Parameters.AddWithValue("@Age", age);
                command.Parameters.AddWithValue("@Email", email);

                command.ExecuteNonQuery();
            }
        }
    }

    static void DeleteEmployee(int id)
    {
        using (SqlConnection connection = new SqlConnection(connectionString))
        {
            connection.Open();

            string deleteQuery = "USE EmployeeDatabase; DELETE FROM Employee WHERE Id = @Id";
            using (SqlCommand command = new SqlCommand(deleteQuery, connection))
            {
                command.Parameters.AddWithValue("@Id", id);

                command.ExecuteNonQuery();
            }
        }
    }

    static void UpdateEmployeeWithValidation(int id)
    {
        // Implement validation logic here before updating the employee information
        // For example, check if the employee exists, validate age, email format, etc.

        // Then update the information
        UpdateEmployee(id, "Updated Name", 35, "updated.email@example.com");
    }
}
