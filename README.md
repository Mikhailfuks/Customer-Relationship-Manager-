using System;
using System.Collections.Generic;

namespace CRM
{
    class Customer
    {
        public int Id { get; set; }
        public string Name { get; set; }
        public string Address { get; set; }
        public string Phone { get; set; }
        public string Email { get; set; }
        public List<Interaction> Interactions { get; set; } = new List<Interaction>();
        public List<Task> Tasks { get; set; } = new List<Task>();

        public Customer(int id, string name, string address, string phone, string email)
        {
            Id = id;
            Name = name;
            Address = address;
            Phone = phone;
            Email = email;
        }
    }

    class Interaction
    {
        public int Id { get; set; }
        public string Type { get; set; } // Call, Email, Meeting, etc.
        public string Description { get; set; }
        public DateTime Date { get; set; }

        public Interaction(int id, string type, string description, DateTime date)
        {
            Id = id;
            Type = type;
            Description = description;
            Date = date;
        }
    }

    class Task
    {
        public int Id { get; set; }
        public string Description { get; set; }
        public DateTime DueDate { get; set; }
        public bool IsCompleted { get; set; }

        public Task(int id, string description, DateTime dueDate, bool isCompleted = false)
        {
            Id = id;
            Description = description;
            DueDate = dueDate;
            IsCompleted = isCompleted;
        }
    }

    class Program
    {
        static List<Customer> customers = new List<Customer>();
        static int nextCustomerId = 1;
        static int nextInteractionId = 1;
        static int nextTaskId = 1;

        static void Main(string[] args)
        {
            Console.WriteLine("Welcome to the CRM System");

            while (true)
            {
                Console.WriteLine("\nChoose an action:");
                Console.WriteLine("1. Add a Customer");
                Console.WriteLine("2. View Customer Details");
                Console.WriteLine("3. Add Interaction");
                Console.WriteLine("4. Add Task");
                Console.WriteLine("5. Exit");

                int choice = Convert.ToInt32(Console.ReadLine());

                switch (choice)
                {
                    case 1:
                        AddCustomer();
                        break;
                    case 2:
                        ViewCustomerDetails();
                        break;
                    case 3:
                        AddInteraction();
                        break;
                    case 4:
                        AddTask();
                        break;
                    case 5:
                        Console.WriteLine("Exiting the CRM...");
                        return;
                    default:
                        Console.WriteLine("Invalid choice. Please try again.");
                        break;
                }
            }
        }

        static void AddCustomer()
        {
            Console.WriteLine("Enter customer name:");
            string name = Console.ReadLine();

            Console.WriteLine("Enter customer address:");
            string address = Console.ReadLine();

            Console.WriteLine("Enter customer phone number:");
            string phone = Console.ReadLine();

            Console.WriteLine("Enter customer email address:");
            string email = Console.ReadLine();

            Customer customer = new Customer(nextCustomerId++, name, address, phone, email);
            customers.Add(customer);
            Console.WriteLine("Customer added successfully.");
        }

        static void ViewCustomerDetails()
        {
            Console.WriteLine("Enter customer ID to view details:");
            int customerId = Convert.ToInt32(Console.ReadLine());

            Customer customer = customers.Find(c => c.Id == customerId);

            if (customer != null)
            {
                Console.WriteLine($"Customer Name: {customer.Name}");
                Console.WriteLine($"Address: {customer.Address}");
                Console.WriteLine($"Phone: {customer.Phone}");
                Console.WriteLine($"Email: {customer.Email}");

                // Display Interactions
                if (customer.Interactions.Count > 0)
                {
                    Console.WriteLine("\nInteractions:");
                    foreach (Interaction interaction in customer.Interactions)
                    {
                        Console.WriteLine($" - {interaction.Type}: {interaction.Description} ({interaction.Date})");
                    }
                }

                // Display Tasks
                if (customer.Tasks.Count > 0)
                {
                    Console.WriteLine("\nTasks:");
                    foreach (Task task in customer.Tasks)
                    {
                        Console.WriteLine($" - {task.Description} (Due: {task.DueDate}) - {(task.IsCompleted ? "Completed" : "Pending")}");
                    }
                }
            }
            else
            {
                Console.WriteLine("Customer not found.");
            }
        }

        static void AddInteraction()
        {
            Console.WriteLine("Enter customer ID:");
            int customerId = Convert.ToInt32(Console.ReadLine());

            Customer customer = customers.Find(c => c.Id == customerId);

            if (customer != null)
            {
                Console.WriteLine("Enter interaction type (Call, Email, Meeting, etc.):");
                string type = Console.ReadLine();

                Console.WriteLine("Enter interaction description:");
                string description = Console.ReadLine();

                Interaction interaction = new Interaction(nextInteractionId++, type, description, DateTime.Now);
                customer.Interactions.Add(interaction);
                Console.WriteLine("Interaction added successfully.");
            }
            else
            {
                Console.WriteLine("Customer not found.");
            }
        }

        static void AddTask()
        {
            Console.WriteLine("Enter customer ID:");
            int customerId = Convert.ToInt32(Console.ReadLine());

            Customer customer = customers.Find(c => c.Id == customerId);

            if (customer != null)
            {
                Console.WriteLine("Enter task description:");
                string description = Console.ReadLine();

                Console.WriteLine("Enter due date (YYYY-MM-DD):");
                DateTime dueDate = DateTime.Parse(Console.ReadLine());

                Task task = new Task(nextTaskId++, description, dueDate);
                customer.Tasks.Add(task);
                Console.WriteLine("Task added successfully.");
            }
            else
            {
                Console.WriteLine("Customer not found.");
            }
        }
    }
}
