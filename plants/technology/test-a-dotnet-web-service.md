+++
author = "Alex Bilson"
date = "2023-02-22"
lastmod = "2023-02-22 16:04:01"
epistemic = "plant"
tags = ["dotnet","testing"]
+++
Here are the basic steps to configure a service that uses `HttpClient` to make HTTP calls which can be unit tested.

{{< notice type=warning >}}
This was developed for a dotnet 7 console app. You may find that your code is more/less verbose.
{{< /notice >}}

## The Service Under Test

First, here's a basic template for a dependency-injected service. You can use these examples to refactor an existing service to inject an `IHttpClientFactory` so we have an interface to mock in our tests.

{{< highlight csharp >}}
namespace Service;
using System.Net.Http;
using System.Text.Json;

public interface ICrawlerClient {
    Task<List<Letter>> Read();
}

public class CrawlerClient : ICrawlerClient {
    private readonly IHttpClientFactory _clientFactory;

    public CrawlerClient(IHttpClientFactory clientFactory)
    {
        _clientFactory = clientFactory;
    }

    public async Task<List<Letter>> ReadLetters() {
        var response = await _clientFactory.CreateClient().GetAsync("http://example.com");

        if (!response.IsSuccessStatusCode) {
            return new List<Letter>();
        }

        var stream = await response.Content.ReadAsStreamAsync();
        var letters = await JsonSerializer.DeserializeAsync<Letters>(stream);
        return letters.financialInstitutionLetters ?? new List<Letter>();
    }
}
{{< /highlight >}}

There's nothing particularly exciting about it. You could probably do better, with less code, but we've got to start somewhere.

If you don't already have the service configuration, here's what I used for my console app.

{{< highlight csharp >}}
var builder = new HostBuilder().ConfigureServices((hostContext, services) => {
	services.AddHttpClient();
	services.AddTransient<ICrawlerClient, CrawlerClient>();
}).UseConsoleLifetime();

var host = builder.Build();

var crawlerClient = host.Services.GetRequiredService<ICrawlerClient>();
{{< /highlight >}}

## The Unit Test

We'll be using XUnit and Moq. I've added an extension from the `Moq.Contrib.HttpClient` NuGet package that simplifies how we replace the `HttpClient` with our own response.

{{< highlight csharp >}}
namespace tests;
using Xunit;
using Moq;
using Moq.Contrib.HttpClient;
using System.IO;
using System.Net.Http;
using System.Text;
using System.Text.Json;

public class ServiceTests
{
    private static Letter Letter = new Letter("test category", "test ordered date", "test date", "test fil", "test href", "test title");

    [Fact]
    public async Task ReadLetters_When_Letter_Exists_Returns_Letter()
    {
        // arrange
        var handler = new Mock<HttpMessageHandler>();
        WhenReturnsALetter(handler);
        var client = GetCrawlerClient(handler);

        // act
        var result = await client.ReadLetters();

        // assert
        HasMatchingLetter(result);
    }

    private void WhenReturnsALetter(Mock<HttpMessageHandler> handler)
    {
        var letters = new Letters(new List<Letter> { Letter });
        handler.SetupRequest(HttpMethod.Get, "http://example.com").ReturnsResponse(ToStream(letters));
    }

    private void HasMatchingLetter(List<Letter> letters) {
        Assert.True(letters.Count() > 0);
        Assert.Equal(Letter, letters[0]);
    }

    private Stream ToStream<T>(T data)
    {
        return new MemoryStream(Encoding.UTF8.GetBytes(JsonSerializer.Serialize<T>(data) ?? ""));
    }

    private CrawlerClient GetCrawlerClient(Mock<HttpMessageHandler> handler)
    {
        return new CrawlerClient(handler.CreateClientFactory());
    }
}
{{< /highlight >}}

The bits which took me the longest to figure out are encapsulated in the `ToStream<T>` and `WhenReturnsALetter` functions. That's because to mimic the real world we must convert our response objects to/from `Stream` objects and I wasn't familiar with how to do that in dotnet at the time.
