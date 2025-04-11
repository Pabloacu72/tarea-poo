# tarea-poo
using System;

namespace SistemaInventario
{
    // 1️⃣ Clase Articulo
    class Articulo
    {
        public string Codigo { get; set; }
        public string Nombre { get; set; }
        public decimal Precio { get; set; }
        public int Stock { get; set; }

        public Articulo(string codigo, string nombre, decimal precio, int stock)
        {
            Codigo = codigo;
            Nombre = nombre;
            Precio = precio;
            Stock = stock;
        }

        public virtual void MostrarInfo()
        {
            Console.WriteLine($"Código: {Codigo}, Nombre: {Nombre}, Precio: ${Precio}, Stock: {Stock}");
        }
    }

    // 2️⃣ Clase Inventario
    class Inventario : Articulo
    {
        private Articulo[] articulos = new Articulo[50];
        private int contador = 0;

        public Inventario() : base("", "", 0, 0) { }

        public void AgregarArticulo(Articulo nuevo)
        {
            if (contador < 50)
            {
                articulos[contador++] = nuevo;
                Console.WriteLine("✅ Artículo agregado correctamente.");
            }
            else
            {
                Console.WriteLine("❌ Inventario lleno.");
            }
        }

        public void MostrarInventario()
        {
            if (contador == 0)
            {
                Console.WriteLine("🗃️ Inventario vacío.");
                return;
            }

            Console.WriteLine("📦 Inventario:");
            for (int i = 0; i < contador; i++)
            {
                articulos[i].MostrarInfo();
            }
        }

        public Articulo BuscarArticulo(string codigo)
        {
            for (int i = 0; i < contador; i++)
            {
                if (articulos[i].Codigo == codigo)
                    return articulos[i];
            }
            return null;
        }

        public void ModificarArticulo(string codigo, decimal nuevoPrecio, int nuevoStock)
        {
            Articulo art = BuscarArticulo(codigo);
            if (art != null)
            {
                art.Precio = nuevoPrecio;
                art.Stock = nuevoStock;
                Console.WriteLine("✅ Artículo modificado.");
            }
            else
            {
                Console.WriteLine("❌ Artículo no encontrado.");
            }
        }
    }

    // 3️⃣ Clase Cliente
    class Cliente
    {
        public string IDCliente { get; set; }
        public string Nombre { get; set; }
        public string Correo { get; set; }

        public Cliente(string idCliente, string nombre, string correo)
        {
            IDCliente = idCliente;
            Nombre = nombre;
            Correo = correo;
        }

        public void MostrarCliente()
        {
            Console.WriteLine($"👤 Cliente - ID: {IDCliente}, Nombre: {Nombre}, Correo: {Correo}");
        }
    }

    // 4️⃣ Clase Program
    class Program
    {
        static void Main(string[] args)
        {
            Inventario inventario = new Inventario();
            Cliente cliente = new Cliente("C001", "Juan Pérez", "juan@example.com");
            bool salir = false;

            while (!salir)
            {
                Console.WriteLine("\n=== MENÚ PRINCIPAL ===");
                Console.WriteLine("1. Agregar Artículo");
                Console.WriteLine("2. Mostrar Inventario");
                Console.WriteLine("3. Buscar Artículo");
                Console.WriteLine("4. Modificar Artículo");
                Console.WriteLine("5. Mostrar Cliente");
                Console.WriteLine("6. Salir");
                Console.Write("Seleccione una opción: ");

                string opcion = Console.ReadLine();
                Console.WriteLine();

                switch (opcion)
                {
                    case "1":
                        Console.Write("Código: ");
                        string codigo = Console.ReadLine();
                        Console.Write("Nombre: ");
                        string nombre = Console.ReadLine();
                        Console.Write("Precio: ");
                        decimal precio = Convert.ToDecimal(Console.ReadLine());
                        Console.Write("Stock: ");
                        int stock = Convert.ToInt32(Console.ReadLine());

                        Articulo nuevo = new Articulo(codigo, nombre, precio, stock);
                        inventario.AgregarArticulo(nuevo);
                        break;

                    case "2":
                        inventario.MostrarInventario();
                        break;

                    case "3":
                        Console.Write("Ingrese código del artículo: ");
                        string codBuscar = Console.ReadLine();
                        Articulo encontrado = inventario.BuscarArticulo(codBuscar);
                        if (encontrado != null)
                            encontrado.MostrarInfo();
                        else
                            Console.WriteLine("❌ Artículo no encontrado.");
                        break;

                    case "4":
                        Console.Write("Código del artículo a modificar: ");
                        string codMod = Console.ReadLine();
                        Console.Write("Nuevo precio: ");
                        decimal nuevoPrecio = Convert.ToDecimal(Console.ReadLine());
                        Console.Write("Nuevo stock: ");
                        int nuevoStock = Convert.ToInt32(Console.ReadLine());

                        inventario.ModificarArticulo(codMod, nuevoPrecio, nuevoStock);
                        break;

                    case "5":
                        cliente.MostrarCliente();
                        break;

                    case "6":
                        salir = true;
                        Console.WriteLine("👋 ¡Hasta luego!");
                        break;

                    default:
                        Console.WriteLine("⚠️ Opción no válida.");
                        break;
                }
            }
        }
    }
}
