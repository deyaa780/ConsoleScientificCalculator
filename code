using System;
using System.Collections.Generic;
using System.Globalization;
using System.Threading;

namespace ScientificCalculator
{
    /// <summary>
    
    /// Supports basic arithmetic, trigonometric, logarithmic, and power operations
    /// </summary>
    class Program
    {
        private static readonly List<string> ValidOperations = new List<string>
        {
            "+", "-", "*", "/", "%",
            "sin", "cos", "tan",
            "pow", "sqrt",
            "log", "ln", "log10"
        };

        private const double Tolerance = 1e-10;

        static void Main()
        {
            // Set culture to handle decimal points consistently
            Thread.CurrentThread.CurrentCulture = CultureInfo.InvariantCulture;

            DisplayWelcomeMessage();
            RunCalculator();
            DisplayGoodbyeMessage();
        }

        /// <summary>
        /// Display welcome message and available operations
        /// </summary>
        private static void DisplayWelcomeMessage()
        {
            Console.WriteLine("╔════════════════════════════════════════╗");
            Console.WriteLine("║        🧮 Scientific Calculator        ║");
            Console.WriteLine("║                                        ║");
            Console.WriteLine("╚════════════════════════════════════════╝");
            Console.WriteLine();
            Console.WriteLine($"📋 Available operations: {string.Join(", ", ValidOperations)}");
            Console.WriteLine();
            Console.WriteLine("📌 Notes:");
            Console.WriteLine("   • Trigonometric functions use degrees");
            Console.WriteLine("   • Use decimal point (.) for decimal numbers");
            Console.WriteLine("   • Type 'help' anytime for operation list");
            Console.WriteLine(new string('─', 50));
        }

        /// <summary>
        /// Main calculator loop
        /// </summary>
        private static void RunCalculator()
        {
            bool continueCalculations = true;

            while (continueCalculations)
            {
                try
                {
                    string operation = GetValidOperation();

                    if (operation == "help")
                    {
                        DisplayHelp();
                        continue;
                    }

                    double[] operands = GetOperands(operation);
                    double result = PerformCalculation(operation, operands);

                    // Only display result if calculation was successful (no error messages)
                    if (!double.IsNaN(result))
                    {
                        DisplayResult(operation, operands, result);
                    }
                }
                catch (FormatException)
                {
                    Console.WriteLine("❌ Error: Please enter valid numbers only.");
                }
                catch (Exception ex)
                {
                    Console.WriteLine($"❌ Unexpected error: {ex.Message}");
                }

                continueCalculations = AskToContinue();
                if (continueCalculations)
                {
                    Console.WriteLine(new string('─', 30));
                }
            }
        }

        /// <summary>
        /// Get valid operation from user input
        /// </summary>
        private static string GetValidOperation()
        {
            while (true)
            {
                Console.Write("\n🔢 Choose operation (or 'help' for list): ");
                string input = Console.ReadLine()?.Trim().ToLower();

                if (string.IsNullOrEmpty(input))
                {
                    Console.WriteLine("❌ Please enter an operation.");
                    continue;
                }

                if (input == "help")
                {
                    return "help";
                }

                if (ValidOperations.Contains(input))
                {
                    return input;
                }

                Console.WriteLine("❌ Unknown operation. Please choose a valid operation.");
                Console.WriteLine($"📋 Available: {string.Join(", ", ValidOperations)}");
            }
        }

        /// <summary>
        /// Display help information
        /// </summary>
        private static void DisplayHelp()
        {
            Console.WriteLine("\n📚 Operation Guide:");
            Console.WriteLine("┌─────────────┬─────────────────────────────────┐");
            Console.WriteLine("│ Operation   │ Description                     │");
            Console.WriteLine("├─────────────┼─────────────────────────────────┤");
            Console.WriteLine("│ +, -, *, /  │ Basic arithmetic                │");
            Console.WriteLine("│ %           │ Modulus (remainder)             │");
            Console.WriteLine("│ pow         │ Power (x^y)                     │");
            Console.WriteLine("│ sqrt        │ Square root                     │");
            Console.WriteLine("│ sin,cos,tan │ Trigonometric (degrees)         │");
            Console.WriteLine("│ log         │ Logarithm with custom base      │");
            Console.WriteLine("│ ln          │ Natural logarithm               │");
            Console.WriteLine("│ log10       │ Base-10 logarithm               │");
            Console.WriteLine("└─────────────┴─────────────────────────────────┘");
        }

        /// <summary>
        /// Get operands based on operation type
        /// </summary>
        private static double[] GetOperands(string operation)
        {
            switch (operation)
            {
                case "sin":
                case "cos":
                case "tan":
                case "sqrt":
                case "ln":
                case "log10":
                    return new[] { GetNumber("Enter a number: ") };

                case "log":
                    return new[]
                    {
                        GetNumber("Enter the base: "),
                        GetNumber("Enter the number: ")
                    };

                case "pow":
                    return new[]
                    {
                        GetNumber("Enter the base: "),
                        GetNumber("Enter the exponent: ")
                    };

                default: // Basic arithmetic operations
                    return new[]
                    {
                        GetNumber("Enter the first number: "),
                        GetNumber("Enter the second number: ")
                    };
            }
        }

        /// <summary>
        /// Get a valid number from user input
        /// </summary>
        private static double GetNumber(string prompt)
        {
            while (true)
            {
                Console.Write(prompt);
                string input = Console.ReadLine();

                if (double.TryParse(input, NumberStyles.Float, CultureInfo.InvariantCulture, out double number))
                {
                    return number;
                }

                Console.WriteLine("❌ Invalid number format. Please try again.");
            }
        }

        /// <summary>
        /// Perform the calculation based on operation and operands
        /// </summary>
        private static double PerformCalculation(string operation, double[] operands)
        {
            switch (operation)
            {
                case "+":
                    return operands[0] + operands[1];

                case "-":
                    return operands[0] - operands[1];

                case "*":
                    return operands[0] * operands[1];

                case "/":
                    if (Math.Abs(operands[1]) < Tolerance)
                    {
                        Console.WriteLine("❌ Cannot divide by zero.");
                        return double.NaN;
                    }
                    return operands[0] / operands[1];

                case "%":
                    if (Math.Abs(operands[1]) < Tolerance)
                    {
                        Console.WriteLine("❌ Cannot modulo by zero.");
                        return double.NaN;
                    }
                    return operands[0] % operands[1];

                case "sin":
                    return CalculateSin(operands[0]);

                case "cos":
                    return CalculateCos(operands[0]);

                case "tan":
                    return CalculateTan(operands[0]);

                case "pow":
                    return CalculatePower(operands[0], operands[1]);

                case "sqrt":
                    return CalculateSquareRoot(operands[0]);

                case "log":
                    return CalculateLogarithm(operands[0], operands[1]);

                case "ln":
                    return CalculateNaturalLog(operands[0]);

                case "log10":
                    return CalculateLog10(operands[0]);

                default:
                    Console.WriteLine("❌ Unknown operation.");
                    return double.NaN;
            }
        }

        // Mathematical operation methods
        private static double CalculateSin(double degrees)
        {
            double normalizedDegrees = NormalizeDegrees(degrees);
            double radians = normalizedDegrees * Math.PI / 180;
            return Math.Sin(radians);
        }

        private static double CalculateCos(double degrees)
        {
            double normalizedDegrees = NormalizeDegrees(degrees);
            double radians = normalizedDegrees * Math.PI / 180;
            return Math.Cos(radians);
        }

        private static double CalculateTan(double degrees)
        {
            double normalizedDegrees = NormalizeDegrees(degrees);
            double radians = normalizedDegrees * Math.PI / 180;

            if (Math.Abs(Math.Cos(radians)) < Tolerance)
            {
                Console.WriteLine($"❌ tan({normalizedDegrees}°) is UNDEFINED!");
                return double.NaN;
            }

            return Math.Tan(radians);
        }

        private static double CalculatePower(double baseValue, double exponent)
        {
            if (exponent == 0)
            {
                return 1;
            }

            if (baseValue == 0 && exponent < 0)
            {
                Console.WriteLine("❌ Undefined: 0 raised to negative power");
                return double.NaN;
            }

            return Math.Pow(baseValue, exponent);
        }

        private static double CalculateSquareRoot(double number)
        {
            if (number < 0)
            {
                Console.WriteLine("❌ Error: Square root of negative number is not real");
                return double.NaN;
            }

            return Math.Sqrt(number);
        }

        private static double CalculateLogarithm(double baseValue, double number)
        {
            if (baseValue <= 0 || Math.Abs(baseValue - 1) < Tolerance)
            {
                Console.WriteLine("❌ Error: Base must be positive and not equal to 1");
                return double.NaN;
            }

            if (number <= 0)
            {
                Console.WriteLine("❌ Error: Number must be positive");
                return double.NaN;
            }

            return Math.Log(number, baseValue);
        }

        private static double CalculateNaturalLog(double number)
        {
            if (number <= 0)
            {
                Console.WriteLine("❌ Error: Number must be positive");
                return double.NaN;
            }

            return Math.Log(number);
        }

        private static double CalculateLog10(double number)
        {
            if (number <= 0)
            {
                Console.WriteLine("❌ Error: Number must be positive");
                return double.NaN;
            }

            return Math.Log10(number);
        }

        /// <summary>
        /// Normalize degrees to 0-360 range
        /// </summary>
        private static double NormalizeDegrees(double degrees)
        {
            return ((degrees % 360) + 360) % 360;
        }

        /// <summary>
        /// Display calculation result
        /// </summary>
        private static void DisplayResult(string operation, double[] operands, double result)
        {
            string formattedResult = FormatResult(result);

            switch (operation)
            {
                case "+":
                    Console.WriteLine($"✅ {operands[0]} + {operands[1]} = {formattedResult}");
                    break;
                case "-":
                    Console.WriteLine($"✅ {operands[0]} - {operands[1]} = {formattedResult}");
                    break;
                case "*":
                    Console.WriteLine($"✅ {operands[0]} × {operands[1]} = {formattedResult}");
                    break;
                case "/":
                    Console.WriteLine($"✅ {operands[0]} ÷ {operands[1]} = {formattedResult}");
                    break;
                case "%":
                    Console.WriteLine($"✅ {operands[0]} % {operands[1]} = {formattedResult}");
                    break;
                case "sin":
                    Console.WriteLine($"✅ sin({NormalizeDegrees(operands[0])}°) = {formattedResult}");
                    break;
                case "cos":
                    Console.WriteLine($"✅ cos({NormalizeDegrees(operands[0])}°) = {formattedResult}");
                    break;
                case "tan":
                    Console.WriteLine($"✅ tan({NormalizeDegrees(operands[0])}°) = {formattedResult}");
                    break;
                case "pow":
                    Console.WriteLine($"✅ {operands[0]}^{operands[1]} = {formattedResult}");
                    break;
                case "sqrt":
                    Console.WriteLine($"✅ √{operands[0]} = {formattedResult}");
                    break;
                case "log":
                    Console.WriteLine($"✅ log_{operands[0]}({operands[1]}) = {formattedResult}");
                    break;
                case "ln":
                    Console.WriteLine($"✅ ln({operands[0]}) = {formattedResult}");
                    break;
                case "log10":
                    Console.WriteLine($"✅ log₁₀({operands[0]}) = {formattedResult}");
                    break;
            }
        }

        /// <summary>
        /// Format result for display
        /// </summary>
        private static string FormatResult(double result)
        {
            // Round very small numbers to zero
            if (Math.Abs(result) < Tolerance)
            {
                return "0";
            }

            // Format large numbers with scientific notation
            if (Math.Abs(result) >= 1e12 || (Math.Abs(result) < 1e-6 && result != 0))
            {
                return result.ToString("E6");
            }

            // Regular formatting with up to 10 decimal places
            return Math.Round(result, 10).ToString();
        }

        /// <summary>
        /// Ask user if they want to continue
        /// </summary>
        private static bool AskToContinue()
        {
            while (true)
            {
                Console.Write("\n🔄 Do you want to perform another calculation? (yes/no): ");
                string userChoice = Console.ReadLine()?.Trim().ToLower();

                switch (userChoice)
                {
                    case "no":
                    case "n":
                        return false;
                    case "yes":
                    case "y":
                        return true;
                    case null:
                    case "":
                        Console.WriteLine("❌ Please enter 'yes' or 'no'.");
                        break;
                    default:
                        Console.WriteLine("❌ Invalid input. Please enter 'yes' or 'no'.");
                        break;
                }
            }
        }

        /// <summary>
        /// Display goodbye message
        /// </summary>
        private static void DisplayGoodbyeMessage()
        {
            Console.WriteLine("\n╔════════════════════════════════════════╗");
            Console.WriteLine("║   ✅ Thank you for using the calculator! ║");
            Console.WriteLine("║                                       ║");
            Console.WriteLine("╚═══════════════════════════════════════╝");
        }
    }
}
