# ⚠️ **RCE / Dangerous Commands – C# (ASP .NET Core)**

---

## 🛠️ **1. Direct OS Command Execution**

**Namespaces & Methods:**

* `System.Diagnostics.Process.Start()`
* `ProcessStartInfo` (with `RedirectStandardOutput` etc.)
* `System.Management.Automation` (PowerShell execution)
* `Microsoft.Win32.Registry` (dangerous if executing shell via registry tricks)

**👉 Also search for:**

* `cmd.exe`, `powershell.exe`, `bash` inside arguments
* `new Process { StartInfo = new ProcessStartInfo(...) }`

---

## 🔑 **2. Eval & Dynamic Code Execution**

**Keywords:**

* `Microsoft.CSharp.CSharpCodeProvider` (`CompileAssemblyFromSource`)
* `System.Reflection.Emit` (`AssemblyBuilder`, `DynamicMethod`)
* `Activator.CreateInstance` (if type is user-controlled)
* `Type.GetType(userInput)` + reflection
* `System.Reflection.MethodInfo.Invoke`

**👉 Also search for:**

* `Assembly.Load`, `Assembly.LoadFrom`, `Assembly.LoadFile`
* `AppDomain.CurrentDomain.Load`
* `Roslyn` APIs (runtime C# compilation)

---

## 📂 **3. File Inclusion / Loading External Assemblies**

**Keywords:**

* `Assembly.LoadFile(userInput)`
* `Assembly.LoadFrom(userInput)`
* `Assembly.ReflectionOnlyLoadFrom`
* `AppDomain.ExecuteAssembly`
* `File.ReadAllText` / `File.ReadAllBytes` + `Assembly.Load`

**👉 Also search for:**

* Any use of `XamlReader.Load` (XAML injection)
* `ConfigurationManager.OpenExeConfiguration(userInput)`

---

## 📦 **4. Serialization / Deserialization**

**Keywords:**

* `BinaryFormatter.Deserialize`
* `SoapFormatter.Deserialize`
* `LosFormatter.Deserialize`
* `NetDataContractSerializer.Deserialize`
* `JavaScriptSerializer.Deserialize`
* `XmlSerializer.Deserialize`
* `DataContractSerializer.ReadObject`

**👉 Also search for:**

* `Newtonsoft.Json.JsonConvert.DeserializeObject` (if type binding enabled → RCE)
* `YamlDotNet.Deserializer`
* `ObjectStateFormatter.Deserialize`

---

## 📝 **5. Dangerous Templating / Expression Injection**

**Keywords:**

* `Eval(userInput)` in ASP.NET WebForms
* `DataBinder.Eval` (if bound to untrusted input)
* Razor: `@Html.Raw(userInput)` (injection risk)
* `Response.Write(string.Format("{0}", userInput))`

**👉 Also search for:**

* `IServiceProvider.GetService` misused with user-controlled types
* Dynamic LINQ (`System.Linq.Dynamic`) → injection leading to RCE in chained cases

---

## 🌐 **6. HTTP Request → Dangerous Execution**

**Keywords:**

* `Request.QueryString` / `Request.Form` → passed into `Process.Start`
* `Request.Params` → used in reflection (`Type.GetType(...)`)
* `Request.Files` → written + `Assembly.Load`

**👉 Also search for:**

* `HttpContext.Current.Server.Execute(userInput)`
* `HttpContext.Current.Response.Write(eval-like logic)`

---

## 📚 **7. Dangerous Libraries / Features**

* **`System.CodeDom.Compiler`** → runtime compilation from strings
* **`Microsoft.CodeAnalysis.CSharp.Scripting`** (`CSharpScript.EvaluateAsync`)
* **`NancyFX`** (older versions had deserialization RCE)
* **`DotNetNuke`**, **Umbraco CMS**, etc. (had known deserialization RCEs)
* **SignalR** (remote invocation vulnerabilities in old versions)
* **Remoting** (`BinaryFormatter` used in .NET Remoting → RCE)
