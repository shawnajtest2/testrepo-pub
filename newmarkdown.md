5. Runtime value injection
  1. Property placeholder
    - ```java
      @Autowired
      Environment env;
      @Bean
      public BlankDisc disc() {
        return new BlankDisc(
          env.getProperty("disc.title"),
          env.getProperty("disc.artist"));
        }
      }
      ```
    - `Environment` class
     * String getProperty(String key)
     * String getProperty(String key, String defaultValue)
     * T getProperty(String key, Class<T> type)
     * T getProperty(String key, Class<T> type, T defaultValue)
     * boolean containsProperty(String key)
     * T getPropertyAsClass(String key, Class<T> type)
     * String[] getActiveProfiles()—Returns an array of active profile names
     * String[] getDefaultProfiles()—Returns an array of default profile names
     * boolean acceptsProfiles(String... profiles)—Returns true if the environment
          supports the given profile(s)

            -   RESOLVING PROPERTY PLACEHOLDERS

                - use:
                  - `c:_title="${disc.title}"`
                  - `@Value("${disc.title}") String title`
            -   configure

                - `<context:property-placeholder />`

                - ```java
                  @Bean
                  public static PropertySourcesPlaceholderConfigurer placeholderConfigurer() {
                    return new PropertySourcesPlaceholderConfigurer();
                  }
                  ```

      2.    SpEL

            - Features:

              - The ability to reference beans by their IDs
              - Invoking methods and accessing properties on objects
              - Mathematical, relational, and logical operations on values
              - Regular expression matching
              - Collection manipulation

            - A few SpEL examples

              ```java
              #{ ... } i.e. #{1}
              #{T(System).currentMillis()}
              #{user.username}
              #{systemProperties['disc.title']}
              ```

            - Expressing literal values

              ```java
              #{3.14159}
              #{9.87E4}
              #{'Hello'}
              #{false}
              ```

            - Referencing beans, properties, and methods

              ```java
              #{user}
              #{user.username}
              #{user.getPassword()}
              #{user.getPassword().toUpperCase()}// occur NullPointerException if password is null
              #{user.getPassword()?.toUpperCase()}// return null if password is null
              ```

            - Working with types in expressions

              ```java
              #{T(java.lang.Math).PI}
              #{T(java.lang.Math).random()}
              ```

            - SpEL Operators

              | Operator type      | Operators                            |
              | ------------------ | ------------------------------------ |
              | Arithmetic         | +, -, *, /, %, ^                     |
              | Comparison         | <, lt, >, gt, ==, eq, <=, le, >=, ge |
              | Logical            | and, or, not, \|                     |
              | Conditional        | ?: (ternary), ?: (Elvis)             |
              | Regular expression | matches                              |

              ```java
              #{2 * T(java.lang.Math).PI * circle.radius}
              #{T(java.lang.Math).PI * circle.radius ^ 2}
              #{disc.title + ' by ' + disc.artist}
              #{counter.total == 100}
              #{scoreboard.score > 1000 ? "Winner!" : "Loser"}
              #{disc.title ?: 'Rattle and Hum'}// if disc.title is null, then the el return "Rattle and Hum"
              ```

            - Evaluating regular expressions

              `#{admin.email matches '[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\\.com'}`

            - Evaluating collections

              ```java
              #{jukebox.songs[4].title}
              #{jukebox.songs[T(java.lang.Math).random() * jukebox.songs.size()].title}
              #{'This is a test'[3]}// return "s"
              #{jukebox.songs.?[artist eq 'Aerosmith']}// .?[] used for selecting
              #{jukebox.songs.^[artist eq 'Aerosmith']}// .^[] and .$[] used for selecting the first/last matching entry.
              #{jukebox.songs.![title]}// .![] used for projection
              // a combined sample
              #{jukebox.songs.?[artist eq 'Aerosmith'].![title]}
              ```
