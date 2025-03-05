# Simplifying React Provider Wrapping with Composition

In React, nesting multiple context providers can lead to provider wrapping hell, making the code hard to read and maintain. Instead of deeply nesting providers like this:

```jsx
<ThemeContext.Provider>
  <UserContext.Provider>
    <QueryClientProvider client={queryClient}>
      <Provider store={store}>
        <IntlProvider locale={usersLocale}>
          <App />
        </IntlProvider>
      </Provider>
    </QueryClientProvider>
  </UserContext.Provider>
</ThemeContext.Provider>
```

You can use a composition pattern to simplify the structure:

1. **Create a utility function** to compose providers:

   ```javascript
   const buildProvidersTree = (componentsWithProps) => {
     const initialComponent = ({ children }) => children;

     return componentsWithProps.reduce(
       (AccumulatedComponents, [Provider, props = {}]) => {
         return ({ children }) => (
           <AccumulatedComponents>
             <Provider {...props}>{children}</Provider>
           </AccumulatedComponents>
         );
       },
       initialComponent
     );
   };
   ```

2. **Define your providers** in a clean, flat structure:

   ```javascript
   const ProvidersTree = buildProvidersTree([
     [ThemeContext.Provider],
     [UserContext.Provider],
     [QueryClientProvider, { client: queryClient }],
     [ReduxProvider, { store }],
     [IntlProvider, { locale: usersLocale }],
   ]);
   ```

3. **Render your app** with the composed providers:
   ```javascript
   const root = createRoot(document.getElementById("root"));
   root.render(
     <ProvidersTree>
       <App />
     </ProvidersTree>
   );
   ```

This approach improves readability, maintainability, and reusability by avoiding deeply nested provider structures.

[source](https://x.com/_georgemoller/status/1896532844337402190)
