Option converter for Newtonsoft!
========================

.. code-block:: fsharp
   
//      Ayrat Hudaygulov, [17.03.19 00:52] https://t.me/fsharp_chat
/// For Some 1 genereates "1", for None generates "null"
type OptionConverter() =    
    inherit JsonConverter()
    
    override x.CanConvert(typ) = typ.IsGenericType && typ.GetGenericTypeDefinition() = typedefof<option<_>>

    override x.WriteJson(writer, value, serializer) =
        let value = 
            if isNull value then null
            else 
                let _,fields = FSharpValue.GetUnionFields(value, value.GetType())
                fields.[0]  
        serializer.Serialize(writer, value)

    override x.ReadJson(reader, typ, existingValue, serializer) =
        let innerType = 
            let innerType = typ.GetGenericArguments().[0]
            if innerType.IsValueType then typedefof<Nullable<_>>.MakeGenericType([|innerType|])
            else innerType        
        
        let cases = Util.getAndCacheUnionCases typ
        if reader.TokenType = JsonToken.Null then FSharpValue.MakeUnion(cases.[0], Array.empty)
        else
            let value = serializer.Deserialize(reader, innerType)
            if isNull value then FSharpValue.MakeUnion(cases.[0], Array.empty)
            else FSharpValue.MakeUnion(cases.[1], [|value|])

 ///......
let s = new JsonSerializer()    
s.Converters.Add(new OptionConverter())

s.Serialize(someObject)