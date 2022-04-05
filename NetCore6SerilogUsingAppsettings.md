```csharp
using Microsoft.Extensions.Configuration;
using Microsoft.Extensions.DependencyInjection;
using Microsoft.Extensions.Hosting;
using Microsoft.Extensions.Logging;
using Serilog;
using System.Reflection;

var host = new HostBuilder()
	.ConfigureAppConfiguration((context, config) =>
	{
		config.SetBasePath(Path.GetDirectoryName(Assembly.GetExecutingAssembly().Location));
		config.AddJsonFile("appsettings.json", optional: true, reloadOnChange: true);
	})
	.ConfigureServices((context, services) =>
	{
		services.AddLogging();
		services.AddTransient<MainService>();
		Log.Logger = new LoggerConfiguration().ReadFrom.Configuration(context.Configuration).CreateLogger();
	})
	.UseSerilog()
	.Build();

try
{
	var service = host.Services.GetRequiredService<MainService>();
	service.Exccute();
}
catch (Exception ex)
{
	Log.Logger.Error(ex.ToString());
}

public class MainService
{
	private readonly ILogger<MainService> _logger;

	public MainService(ILogger<MainService> logger)
	{
		_logger = logger;
	}

	public void Exccute()
	{
		_logger.LogInformation("DIO:「THE WORLD!」");
	}
}
```
