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

## Generic

  * [Haskell Wiki](https://wiki.haskell.org/Generics) - options for generics
    * Template seems to be the most powerful one
    * GHC.Generics
    * Template
    * others

## Template

* [Template Haskell](https://ocharles.org.uk/blog/guest-posts/2014-12-22-template-haskell.html)
  * [Template examples](http://hackage.haskell.org/package/derive)
  * Compiler
  * Debugging
    * stack build --ghc-options=-ddump-splices
    * stack ghci --ghc-options=-ddump-splices
    * ``` >>= putStrLn . pprint ```
  * Splice: splice can occur in place of
    1. an expression; the spliced expression must have type Q Exp
    1. a pattern; the spliced pattern must have type Q Pat
    1. a type; the spliced expression must have type Q Type
    1. a list of declarations at top level; the spliced expression must have type Q [Dec]

  * Code Generation
    * Q monad (Quotation Monad)
      - PatQ ```type PatQ = Q Pat```
    ```haskell
    > runQ [|1+2|]
    InfixE (Just (LitE (IntegerL 1))) (VarE GHC.Num.+) (Just (LitE (IntegerL 2)))
    ```
    * Using : func = lambda (args) (body)
      * args => PatQ
      * body => ExpQ
    * Quotation
      * Pattern Q -> PatQ
      * Expression Q -> ExpQ
    * Codes:
    ```haskell
    -- Function
    varP    -- varP :: Name -> PatQ
    mkName  -- mkName :: String -> Name
    infixApp -- infixApp :: ExpQ -> ExpQ -> ExpQ -> ExpQ
    stringE -- stringE :: String -> ExpQ
    appE -- appE :: ExpQ -> ExpQ -> ExpQ
    varE -- varE :: Name -> ExpQ
    lamE -- lamE :: [PatQ] -> ExpQ -> ExpQ
    -- Types
    ExpQ    -- type ExpQ = Q Exp
    ```
    * import Language.Haskell.TH
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
  * DSL (domain specific language)
    * quasi-quotation (mostly used for DSL)
      * {-# LANGUAGE QuasiQuotes #-}
      ```haskell
      ["quoter"|...|]
      ```
  * reification (reify)
    * queries the state of Haskell __name__ and get info about them
      * info depends on whether __type context__ or __expression context__
    * **reify** returns data type called **info**
    * expression context -> single quote(')
      ```haskell
      $(stringE . show =<< reify 'Bool)
      ```
    * type context -> double quote('')
      ```haskell
      $(stringE . show =<< reify ''Bool)
      ```


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

* ```{-# LANGUAGE OverloadedStrings #-}```
* ```{-# LANGUAGE DeriveGenerics #-}```

## Compiler option on file
* ```{-# OPTIONS_GHC -Wall #-}```

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

## Documentation

### Haddock

* [Haddock documentation] (https://www.haskell.org/haddock/doc/html/ch03s03.html)

## Troubleshooting

### VIM seems stuck when saving .hs files

* hdevtools is stuck
  * try reinstalling hdevtools using ```stack install hdevtools```
  * make sure path points to the right version of hdevtools

