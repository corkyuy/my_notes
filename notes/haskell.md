# Haskell Notes

## Info

* [Typeclassopedia](https://wiki.haskell.org/Typeclassopedia)
* [Documentation](https://downloads.haskell.org/~ghc/8.0.2/docs/html/users_guide/)

## Codes

fibs
```haskell
fibs :: [Integer]
fibs = 0 : 1 : zipWith (+) fibs (tail fibs)
```

## Template

* [Template Haskell](https://ocharles.org.uk/blog/guest-posts/2014-12-22-template-haskell.html)
  * Code Generation
    * Q monad (Quotation Monad)
    ```haskell
    > runQ [|1+2|]
    InfixE (Just (LitE (IntegerL 1))) (VarE GHC.Num.+) (Just (LitE (IntegerL 2)))
    ```
    * Language.Haskell.TH
      * data Lit = CharL Char ...
    * {-# LANGUAGE TemplateHaskellQuotes #-}
    * {-# LANGUAGE TemplateHaskell #-}
      * Expression AST
        ```haskell
        [|...|]
        OR
        [e|...|]

        > runQ [| 1 + 2 |]
        ```
      * Top-Level Declaration AST
        ```haskell
        [d|...|]

        > runQ [d| x=5 |]
        ```
      * Type AST
        ```haskell
        [t|...|]

        > runQ [t| Int |]
        ```
      * Pattern AST
        ```haskell
        [p|...|]

        > runQ [t|(x,y)|]
        ```
      * quasi-quotation (mostly used for DSL)
        * {-# LANGUAGE QuasiQuotes #-}
        ```haskell
        ["quoter"|...|]
        ```



  * DSL (domain specific language)

## Libraries

* Data.Text -> Unicode support
* Data.Aeson -> JSON
* Data.ByteString -> sequence of bytes (high speed performance)

* GHC.Generics ->
  ``` Haskell
  import GHC.Generics

  data Person = Person
    { name :: Text
    , age :: Int
    } deriving ( Generic, Show )

  instance FromJSON Person
    -- generate automagically
  instance ToJSON Person
  ```

## Extensions

* OverloadedStrings
* DeriveGenerics

## GHCI

* Enable Extensions
  * :set -XOverloadedStrings
* Multiline
```
:{
let fibs::[Integer]
  fibs = 0 : 1 : zipWith (+) fibs (tail fibs)
:}
```

## Troubleshooting

### VIM seems stuck when saving .hs files

* hdevtools is stuck
  * try reinstalling hdevtools using stack install hdevtools
  * make sure path points to the right version of hdevtools
