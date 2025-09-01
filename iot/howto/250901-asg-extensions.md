# How to Write a Custom Ash Framework Extension

Writing a custom extension for the Ash Framework involves using the `Spark.Dsl.Extension` to define and patch a new set of declarative constructs onto Ash resources. This process leverages Spark, the powerful DSL-building library that underpins Ash. The key to creating an extension is to use `Spark.Dsl.Extension` and create "patches" that add or modify Ash's DSL entities.

see [hexdoc](https://hexdocs.pm/ash/writing-extensions.html)

## Example: A Custom `log_events` Extension

Let's walk through an example of a simple extension that adds a `log_events` block to Ash resources, which could be used to enable event logging.

### Step 1: Create the DSL Module

The core of your extension is a module that defines the new DSL. In this example, it defines a new `event` entity to go inside a `log_events` block.

```
defmodule MyApp.Ash.LogEvents.Dsl do
  use Spark.Dsl.Extension

  @event %Spark.Dsl.Entity{
    name: :event,
    target: __MODULE__.Event,
    describe: "Log an event with a given name.",
    args: [:name],
    schema: [
      name: [
        type: :atom,
        required: true,
        doc: "The name of the event to log."
      ],
      description: [
        type: :string,
        default: "A custom event.",
        doc: "A description of the event."
      ]
    ]
  }

  @log_events %Spark.Dsl.Entity{
    name: :log_events,
    target: __MODULE__,
    describe: "Defines a list of events to log.",
    entities: [@event],
    schema: [
      enabled: [
        type: :boolean,
        default: true
      ]
    ]
  }

  def dsl_patches do
    [
      %Spark.Dsl.Patch.AddEntity{
        section_path: [:resource],
        entity: @log_events
      }
    ]
  end
end
```

**Explanation:**

*   `use Spark.Dsl.Extension`: This macro provides the necessary boilerplate for creating DSL extensions.
*   `@event` and `@log_events`: These are `Spark.Dsl.Entity` structs that define the shape of your new DSL.
   - `name`: The name of the DSL block or keyword.
   - `target`: The module that will receive the compiled DSL state.
   - `schema`: The options available within the DSL block, defined using a keyword list of types.
*   `dsl_patches/0`: This callback returns a list of patches to apply to the Ash resource DSL.
  *   `Spark.Dsl.Patch.AddEntity`: Adds your new DSL entity to the specified `section_path`. In this case, `[:resource]` means adding the `log_events` block directly inside `use Ash.Resource`.

### Step 2: Define the Target Modules

The `target` key in your DSL entities points to modules that will hold the compiled configuration. Ash expects the configuration to be compiled into a struct.

```elixir
defmodule MyApp.Ash.LogEvents do
  defstruct enabled: true, events: []
end

defmodule MyApp.Ash.LogEvents.Event do
  defstruct name: nil, description: nil
end
```

### Step 3: Use the Extension in an Ash Resource

With your extension defined, you can now add it to an Ash resource by using the `extensions` option.

```elixir
defmodule MyApp.Accounts.User do
  use Ash.Resource,
    extensions: [MyApp.Ash.LogEvents.Dsl]

  # ... standard resource configuration

  log_events do
    event :user_created, description: "A new user was created."
    event :user_updated, description: "A user's profile was updated."
  end
end
```

### Step 4: Implement the Runtime Logic

An extension is purely declarative at this point. You need to write runtime code that reads the extension's configuration and performs an action. For our `log_events` example, this could be an Ash change that reads the configured events and logs them after a successful action.

```elixir
defmodule MyApp.Ash.Changes.LogEvents do
  use Ash.Resource.Change

  def change(changeset, _opts, _context) do
    # Read the extension configuration from the resource.
    case Ash.Resource.Info.extensions(changeset.resource)
    |> Keyword.get(MyApp.Ash.LogEvents.Dsl) do
      %MyApp.Ash.LogEvents{events: events, enabled: true} ->
        # Log the events. This is just an example.
        for event <- events do
          IO.puts("Logging event: #{inspect(event)}")
        end
        :ok

      _ ->
        :ok
    end
  end
end
```

You would then add this change to your resource's actions:

```elixir
defmodule MyApp.Accounts.User do
  # ...

  actions do
    create :create do
      change {MyApp.Ash.Changes.LogEvents, nil}
    end
  end
end
```

### Key Takeaways for Custom Ash Extensions

*   **DSL with Spark**: All Ash extensions rely on Spark, a library for creating Elixir DSLs. You define the structure of your DSL using `Spark.Dsl.Entity` structs and apply them using `Spark.Dsl.Patch`.
*   **Compile-time transformations**: Extensions are most powerful when they perform compile-time resource transformations. By defining a custom `transform` callback in your `Spark.Dsl.Entity`, you can modify the resource's state before the code is compiled.
*   **Use `Spark.Dsl.Extension`**: The `use Spark.Dsl.Extension` macro is the standard entry point for building new extensions.
*   **Runtime behavior**: The declarative DSL is just one part. You also need a separate module (like a change, preparation, or data_layer) to contain the actual runtime logic that interacts with your custom DSL configuration.
*   **Target modules**: The configuration declared in your DSL is compiled into structs. You need to define these target modules (e.g., `MyApp.Ash.LogEvents`) to store the configuration.

---

# How to write ash framework custom extension

Writing custom extensions in Ash Framework primarily involves using the `Spark.Dsl.Extension` module and leveraging Spark's DSL capabilities to define new DSL entities or patch existing ones.

Here's a general approach to writing an Ash Framework custom extension:

## Define your Extension Module

Create a new module for your extension and use `Spark.Dsl.Extension`. This provides the necessary macros and functions for defining DSL extensions.

```elixir
defmodule MyApp.MyExtension do
  use Spark.Dsl.Extension
  # ... your extension definition
end
```

## Define New DSL Entities (if needed)

If your extension introduces new DSL keywords or sections, you'll define them using `Spark.Dsl.Entity`. This involves specifying the entity's name, arguments, target module (e.g., `Ash.Resource.Attribute`), and a schema to validate its options.

```elixir
defmodule MyApp.MyExtension do
  use Spark.Dsl.Extension

  @my_custom_entity %Spark.Dsl.Entity{
    name: :my_custom_entity,
    args: [:name, :options],
    target: Ash.Resource.Attribute, # Or another Ash component
    schema: Spark.Dsl.Extension.Schema.new(
      # Define your options schema here
      option_one: :string,
      option_two: :boolean
    )
  }

  # ... other DSL definitions
end
```

## Patch Existing DSL Entities (if needed)

If your extension modifies or extends existing Ash DSL, you'll use `Spark.Dsl.Patch` to add new entities, modify existing ones, or remove them. Common patches include `Spark.Dsl.Patch.AddEntity` to inject new DSL entities into existing sections.

```elixir
defmodule MyApp.MyExtension do
  use Spark.Dsl.Extension

  # ... @my_custom_entity definition

  @attributes_patch %Spark.Dsl.Patch.AddEntity{
    section_path: [:attributes], # Target the :attributes section of a resource
    entity: @my_custom_entity
  }

  # Apply the patches
  dsl_patches [@attributes_patch]
end
```

## Implement the Logic

The DSL definition primarily focuses on how the extension is declared in your Ash resources. The actual logic that the extension performs needs to be implemented separately. This might involve:

-   **Custom types:** Define new Ash types if your extension introduces new data types.
-   **Transformers:** Create functions that transform the DSL options or arguments into a usable format.
-   **Hooks or Callbacks:** Integrate with Ash's lifecycle hooks or define callbacks that are triggered at specific points (e.g., before an action runs).

## Use the Extension in Your Resources

Once your extension is defined, you can use it in your Ash resources by adding `use MyApp.MyExtension` to the resource module.

```elixir
defmodule MyApp.MyResource do
  use Ash.Resource
  use MyApp.MyExtension # Use your custom extension

  attributes do
    # ...
  end

  my_custom_entity :my_field, option_one: "value", option_two: true
end
```

By following these steps and utilizing Spark's powerful DSL capabilities, you can create custom Ash Framework extensions to tailor Ash to your specific application needs. For more complex extensions, exploring existing Ash extensions on HexDocs or GitHub can provide valuable insights.

---

# How Ash Framework enables quick app development

Ash Framework significantly accelerates application development in Elixir by offering a declarative, resource-oriented approach to building backends. It streamlines the creation of APIs, database schemas, and background jobs through its focus on "resources."

## Key aspects enabling quick app development with Ash

### Declarative Resource Definition

Ash allows developers to define application components as resources, which can represent anything from database tables to external APIs. This declarative approach means defining what the application should do rather than how it should do it, reducing boilerplate code and speeding up setup.

### Built-in Functionality

Ash provides a comprehensive suite of built-in features that are common in many applications, such as:

-   **Data Manipulation:** Filtering, sorting, pagination, calculations, and aggregations are handled with minimal configuration.
-   **Authorization:** Policy-based authorization is integrated, allowing fine-grained control over resource access.
-   **Pub/Sub:** Publish/subscribe mechanisms are available for real-time updates and event handling.
-   **API Generation:** Extensions like `AshGraphql` and `AshJsonApi` can automatically generate robust APIs based on resource definitions, significantly reducing the time spent on API development.

### Reduced Boilerplate

By modeling applications as resources and leveraging Ash's built-in capabilities, developers can avoid writing repetitive code for common tasks, leading to faster development cycles.

### Extensibility

While providing extensive out-of-the-box functionality, Ash is also designed to be highly extensible, allowing developers to build custom extensions and tailor the framework to specific needs without sacrificing development speed.

In essence, Ash Framework enables rapid application development by providing a high-level, declarative architecture that simplifies the creation and management of application backends, allowing developers to focus on core business logic rather than infrastructure details.

---

# Quick app dev with Ash Framework

Ash Framework enables quick Elixir application development by using a declarative, resource-oriented approach to generate common backend components from your data models. Instead of writing boilerplate code for APIs, database migrations, and business logic, you define "resources" and their "actions," and Ash generates the infrastructure for you.

## How Ash accelerates development

### Declarative, resource-oriented modeling

You model your application's domain using Ash's Domain Specific Languages (DSLs). You define resources (the "nouns" of your application, like `User` or `Post`), their attributes, relationships, and the actions that can be performed on them.

### Automatic generation of infrastructure

From your resource definitions, Ash extensions derive infrastructure with minimal configuration, including:

-   Database migrations
-   LiveView forms for Phoenix
-   JSON and GraphQL APIs
-   Robust authorization policies

### Reduced boilerplate

Ash eliminates the need to manually write repetitive code for contexts, persistence, and authorization, which are typical tasks in an MVC framework like Phoenix.

### Focus on business logic

By handling the "how" of infrastructure, Ash allows you to focus on the "what"—the unique business logic of your application.

### Integrates with the Elixir ecosystem

Ash is designed to work with popular tools such as Phoenix, LiveView, and Ecto/Postgres, so you can leverage the broader Elixir ecosystem.

## Key concepts for rapid development with Ash

-   **Resources:** At the core of Ash, a resource models an application entity. You use a DSL to define its attributes, relationships, and actions.
-   **Actions:** These define how to interact with a resource (e.g., create, read, update, destroy). You can also define custom actions for specific business logic.
-   **Domains:** Resources are organized into domains, which group related functionality.
-   **Extensions:** Ash's plug-and-play architecture uses extensions to add functionality. For example, `AshPostgres` provides a data layer, `AshPhoenix` integrates with Phoenix, and `AshJsonApi` generates a JSON API.

## Example: A basic blog application

You can quickly build a blog using Ash with the following steps:

1.  **Set up the project:** Use the `igniter` toolkit to create a new Phoenix project with Ash and `AshPostgres` pre-installed.
2.  **Generate a `Post` resource:** Use a generator to create a `Post` resource, specifying attributes like `title` and `body`.

    ```bash
    mix ash.gen.resource AshDemo.Blog.Post --extend postgres
    ```

3.  **Define attributes and actions:** Fill in the resource definition with attributes and set up default actions like `create`, `read`, and `update`.

    ```elixir
    defmodule AshDemo.Blog.Post do
      use Ash.Resource

      # ...

      actions do
        defaults [:create, :read, :update, :destroy]
        default_accept [:title, :body]
      end

      attributes do
        uuid_primary_key :id
        attribute :title, :string
        attribute :body, :string
      end

      # ...
    end
    ```

4.  **Generate a LiveView:** Use the Ash Phoenix generator to create a LiveView with forms for the `Post` resource.

    ```bash
    mix ash_phoenix.gen.live --domain AshDemo.Blog --resource AshDemo.Blog.Post
    ```

5.  **Run the application:** Start your server and navigate to the generated route to see the fully functional, scaffolded LiveView for managing blog posts.

This process demonstrates how Ash's declarative approach and powerful generators allow you to create a functional CRUD application in minutes.

---

# Ash Framework Extensibility

Ash Framework, an application development framework for Elixir, is designed with a strong emphasis on extensibility, enabling developers to build custom extensions and tailor the framework to their specific needs.

## Key aspects of Ash's extensibility

-   **Declarative Design and Resources:** Ash utilizes a declarative approach, where developers define "Resources" to model the core of their application, such as database schemas, API endpoints, and business logic. This declarative nature makes it easier to extend and modify behavior consistently across the application.
-   **Built-in Extensibility Mechanisms:** Ash provides a suite of first-party extensions for common functionalities (e.g., PostgreSQL, Phoenix, Oban, authentication, state machines) and also offers a toolkit for building custom extensions.
-   **Escape Hatches and Overrides:** The framework includes various "escape hatches" and mechanisms to override default behavior, ranging from small adjustments to significant modifications of internal processes. This allows developers to integrate custom logic or adapt Ash to unique requirements without being constrained by the framework's default implementations.
-   **Integration with Elixir Ecosystem:** Ash is built on Elixir and integrates seamlessly with existing Elixir libraries and tools, enabling developers to leverage the broader Elixir ecosystem for custom functionalities or integrations not directly provided by Ash.
-   **DSL Sections for Configurability:** Extensions can be made configurable through the use of DSL sections, allowing developers to define options and parameters for their custom extensions, making them more flexible and reusable.

In essence, Ash's design prioritizes flexibility, allowing developers to extend its capabilities and adapt it to diverse application requirements, fostering a robust and customizable development environment.
