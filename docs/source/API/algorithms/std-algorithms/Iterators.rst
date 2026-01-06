イテレータ
=========

.. role:: cpp(code)
    :language: cpp

``Kokkos::Experimental::{begin, cbegin, end, cend}``
----------------------------------------------------

ヘッダーファイル: ``<Kokkos_StdAlgorithms.hpp>``


.. cpp:function:: template <class DataType, class... Properties> KOKKOS_INLINE_FUNCTION auto begin(const Kokkos::View<DataType, Properties...>& view);

   ``view``の先頭へKokkos **random access** イテレータを返します。

.. cpp:function:: template <class DataType, class... Properties> KOKKOS_INLINE_FUNCTION auto cbegin(const Kokkos::View<DataType, Properties...>& view);

   ``view``の先頭へKokkos const修飾**random access** イテレータを返します。

.. cpp:function:: template <class DataType, class... Properties> KOKKOS_INLINE_FUNCTION auto end(const Kokkos::View<DataType, Properties...>& view);

   ``view``の要素の末尾を過ぎた位置までKokkos **random access** イテレータを返します。


.. cpp:function:: template <class DataType, class... Properties> KOKKOS_INLINE_FUNCTION auto cend(const Kokkos::View<DataType, Properties...>& view);

   ``view``の要素の末尾を過ぎた位置までKokkos **random access** イテレータを返します。

注意事項
~~~~~

* 返されたイテレーターは、パフォーマンス上の理由からランダムアクセスです。

* 各関数内ではviewそのものを変更しないため、``view``は``const``として扱われます: 返されたイテレータは、構造を変更せずにview上で動作します。

* イテレータの参照解除は、``view``がアクセス可能な実行空間内で行う必要があります。

パラメータと要件
~~~~~~~~~~~~~~~~~~~~~~~~~~~

* ``view``: ``LayoutLeft``、``LayoutRight``、または``LayoutStride`` を持つランク1 viewでなければなりません。

例
~~~~~~~

.. code-block:: cpp

    namespace KE = Kokkos::Experimental;
    using view_type = Kokkos::View<int*>;
    view_type a("a", 15);

    auto it = KE::begin(a);
    // （適切な実行空間内で）間接参照された場合、`a`の内容を変更できます。

    auto itc = KE::cbegin(a);
    // 適切な実行空間内で）参照解除された場合、`a`の内容を読み取ることのみが可能です。

------------------

``Kokkos::Experimental::distance``
----------------------------------

.. cpp:function:: template <class IteratorType> KOKKOS_INLINE_FUNCTION constexpr typename IteratorType::difference_type distance(IteratorType first, IteratorType last);

   ``first``から``last``まで移動するのに必要なステップ数を返します。

Parameters and Requirements
~~~~~~~~~~~~~~~~~~~~~~~~~~~

* ``first, last``: 距離を計算するための範囲

リターン
~~~~~~

``first``から``last``まで移動するのに必要なステップ数。
ランダムアクセスイテレータを使用する場合、値が負になることがあります。


例
~~~~~~~

.. code-block:: cpp

    namespace KE = Kokkos::Experimental;
    Kokkos::View<double*> a("a", 13);

    auto it1 = KE::begin(a);
    auto it2 = it1 + 4;
    const auto stepsA = KE::distance(it1, it2);
    // ステップAは4である必要があります。

    const auto stepsB = KE::distance(it2, it1);
    // ステップBは-4である必要があります。

------------------

``Kokkos::Experimental::iter_swap``
-----------------------------------

.. cpp:function:: template <class IteratorType> void iter_swap(IteratorType first, IteratorType last);

指定された反復子が指す要素の値を入れ替えます。
   
パラメータおよび要件
~~~~~~~~~~~~~~~~~~~~~~~~~~~

* ``first, last``: 入れ替えるためのイテレータ

注意事項
~~~~~

現在、操作がデフォルトの実行スペースで実行されるため、APIには実行スペースパラメータがありません*。この操作はデフォルトの実行領域をフェンスします。

リターン
~~~~~~

無し

例
~~~~~~~

.. code-block:: cpp

    namespace KE = Kokkos::Experimental;
    Kokkos::View<double*> a("a", 13);

    auto it1 = KE::begin(a);
    auto it2 = it1 + 4;
    KE::swap(it1, it2);
