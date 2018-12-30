=======
Array
=======

.. image:: /_images/array.png
    :height: 145px

`Массивы в F# <https://docs.microsoft.com/ru-ru/dotnet/fsharp/language-reference/arrays>`_
 * имеют фиксированную длину. Кроме ResizeArray - это всего лишь псевдоним обычного .net списка:
 .. code-block:: fsharp
   
      type ResizeArray<'T> = System.Collections.Generic.List<'T>
 
 * элементы массива изменяемы (mutable)
 * могут быть созданы различными способами. Вот лишь некоторые:
 .. code-block:: fsharp
   
      let array1 = [| 1; 2; 3 |]
      let array2 = 
              [| 
                  1 
                  2
               |]
      let array3 = [| for i in 1 .. 10 -> i * i |]
 * индексируются с нуля (zero based). Примеры доступа к одному или нескольким элементам массива:
 .. code-block:: fsharp
   
      let array1 = [| 1; 2; 3; 4; 5; |]
      let firstItem=array.[0] // первый элемент
      let subarray1=array.[1..3] // "отрезок" между индексами 2;3;4;
      let subarray2=array.[..3] // с начала и до указанного индекса 1;2;3;4;
      let subarray3=array.[3..] // с указанного индекса и до конца 4;5
 