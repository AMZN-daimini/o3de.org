---
linkTitle: Local Testing
title: AWS GameLift Gem Local Testing
description: "Learn how to test locally with the AWS GameLift Gem in O3DE"
toc: true
weight: 600
---

In this topic, you will learn how to test and verify AWS GameLift Gem feature integrations on your local machine by using **GameLift Local**. 

GameLift Local is a command line tool that starts a self-contained version of the managed Amazon GameLift service. GameLift Local also provides a running event log of server process initialization, health checks, and API calls and responses. GameLift Local recognizes a subset of the AWS SDK actions for GameLift. You can make calls from the AWS CLI or from your game client. All API actions perform locally, just as they do in the GameLift web service. For more information, refer to [Testing Your Integration](https://docs.aws.amazon.com/gamelift/latest/developerguide/integration-testing-local.html#integration-testing-local-start) in the Amazon GameLift Developer Guide.

{{< caution >}}  
GameLift Local does not currently support testing of FlexMatch features.
{{< /caution >}}

## 1. Start GameLift Local

With GameLift Local, you can verify the following: (Refer to [Set Up GameLift Local](https://docs.aws.amazon.com/gamelift/latest/developerguide/integration-testing-local.html#integration-testing-local-start))

*   Your game server is correctly integrated with the Server SDK and is properly communicating with the GameLift service to start new game sessions, accept new players, and report health and status.
    
*   Your game client is correctly integrated with the AWS SDK for GameLift and is able to retrieve information on existing game sessions, start new game sessions, join players to games and connect to the game session.
    

After downloading GameLift Local, you should be able to start it by (please sure you are passing the same port while starting game, as GameLift client endpoint needs to use the same port)

```shell

$ java -jar GameLiftLocal.jar -p 9080

```

## 2. Set `sv_gameLiftEnabled` to `true`

When you are ready to test with GameLift, opt-in by setting the `sv_gameLiftEnabled` CVAR to `true`. This CVAR is off by default to facilitate local testing prior to the integration of the GameLift Server SDK into your game server.

You can set this value by creating a settings registry file named `commands.MyProject_serverlauncher.setreg` in the `MyProject/Registry` folder with the following contents:

```json
{
    "Amazon":
    {
        "AzCore":
        {
            "Runtime":
            {
                "ConsoleCommands":
                {
                    "sv_gameLiftEnabled": "true"
                } 
            } 
        } 
    } 
}
```

This lets your game server use GameLift local when running game or simulation mode in the Editor, or when running the server launcher directly (see next step).

## 3. Start Server

Make sure you have a cmake build target for your server, like YourProject.ServerLauncher. and build the application for your local testing.

You should be able to find application under build bin folder, and launch the server by

```shell

$ bin\profile\YourProject.ServerLauncher.exe

```

## 4. Start Game

Make sure you have a cmake build target for your game, like YourProject.GameLauncher, and build the application for your local testing.

You should be able to find application under build bin folder, and launch the game by (please make sure you are passing the same port while starting GameLift Local)

```cpp

// Override GameLift endpoint to local host

$ bin\profile\YourProject.GameLauncher.exe --cl_gameliftLocalEndpoint "http://localhost:9080"

```

## 5. Test Game and Server

Testing steps and instructions really depend on your own project, but please make sure your testing can cover CreateSession, JoinSession, LeaveSession and DestroySession use case.

You can also verify interactions and logs with the GameLift Local tool. For more information, refer to [Test a Game Server and Client](https://docs.aws.amazon.com/gamelift/latest/developerguide/integration-testing-local.html#integration-testing-local-client) in the Amazon GameLift Developer Guide.
