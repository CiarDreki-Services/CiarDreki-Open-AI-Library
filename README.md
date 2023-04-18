```
____ _ ____ ____ ___  ____ ____ _  _ _    ____ ___  ____ _  _    ____ _    _    _ ___  ____ ____ ____ _   _
|    | |__| |__/ |  \ |__/ |___ |_/  |    |  | |__] |___ |\ | __ |__| |    |    | |__] |__/ |__| |__/  \_/  
|___ | |  | |  \ |__/ |  \ |___ | \_ |    |__| |    |___ | \|    |  | |    |___ | |__] |  \ |  | |  \   |   
```                                                                                                        
------------------------------------------------------------------------------------------------------------

CiarDreki Services has designed this library to be easy to use and encompasses ALL current Open AI Apis.
Many of the widely available Open AI libraries on the market are lacking something, whether it's the Moderation API,
the File Upload API, or the training APIs, nothing is ever fully fleshed out and ready to go.

CiarDreki Open-AI Library is a simple to use library that you can add to any .Net 6+ program that allows you to easily
connect to the OpenAI API without having to worry about URI construction or JSON Body content. All of that is already
added to the library!

# USAGE:
First, we create an instance of the OpenAI class, passing it our Open AI API key. CiarDreki Open-AI Library also houses
the CiarDreki Services Encrypt/Decrypt tool, which you can use to Encrypt a file containing your API key with a secure
password of your choosing:
```csharp
var openAI = new OpenAI("Your Open AI API Key");
```
After we've created an instance of the OpenAI class, we can start returning results. Each Open AI EndPoint is listed as a method inside the Library.

## Completions

```csharp
CompletionRequest request = new()
{
    Model = Model.DefaultModel,
    Prompt = "Take me down to the paradise city"
};

//Returns: "Where the grass is green and the girls are pretty."
var completion = await openAI.Completions.CreateCompletionAsync(request);
Console.WriteLine(completion.ToString());
```

## Chat
```csharp
var chatBot = openAI.Chat.CreateConversation();

//Give the bot a personality or tell it how to react.
chatBot.AppendSystemMessage("You work for Footlocker, you'll only answer people if the question is related to shoes, otherwise you'll respond with a: \"We're sorry, but we don't carry those things!\"");

//Get user question
while (true)
{
    var userQuestion = Console.ReadLine();
    chatBot.AppendUserInput(userQuestion);
    Console.WriteLine(await chatBot.GetResponseFromChatbotAsync());

}
```

## Embedding
```csharp
var result = await openAI.Embeddings.CreateEmbeddingAsync("The Dead Tell No Tale...");
var embedded = result.Data[0].Embedding.ToList();
// Returns a vector of floats
```

## Files and Fine-Tuning
```csharp
// First we must load a file to OpenAI using the UploadFile API.
var filePath = "trainingData.jsonl";
var trainingFile = await openAI.Files.UploadFileAsync(filePath);

// We can then use the trainingFile ID to create a Fine Tune job.
var fineTune = await openAI.FineTune.CreateFineTuneAsync(trainingFile.Id);
```
## Images
```csharp
// Take in a prompt
var prompt = Console.ReadLine();
//Initiate an Image Request object.
var imageRequest = new ImageRequest()
{
    NumOfImages= 1,
    Size = ImageSize._256,
    ResponseFormat = ImageResponseFormat.Url, //or ImageResponseFormat.B64_json
    Prompt = prompt
};
// Create the image and get the data.
var image = await openAI.ImageGenerations.CreateImageAsync(imageRequest);

Console.WriteLine(image.Data[0].Url);
```
