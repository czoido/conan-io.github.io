---
layout: post
comments: false
title: "An introduction to CGAL"
---

El c3i está en constante evolución y acabamos de sacar CGAL 5.0 que es muy usada bla bla bla

### What is CGAL

[CGAL](https://www.cgal.org/) is:

From the web:

CGAL is a software project that provides easy access to efficient and reliable geometric algorithms in the form of a C++ library. CGAL is used in various areas needing geometric computation, such as geographic information systems, computer aided design, molecular biology, medical imaging, computer graphics, and robotics.

The library offers data structures and algorithms like triangulations, Voronoi diagrams, Boolean operations on polygons and polyhedra, point set processing, arrangements of curves, surface and volume mesh generation, geometry processing, alpha shapes, convex hull algorithms, shape reconstruction, AABB and KD trees...

Learn more about CGAL by browsing through the Package Overview.

CGAL tiene un montón de paquetes diferentes: 
     - Arithmetic and Algebra
     - Combinatorial Algorithms
     - Geometry Kernels (qué es un kernel???)
     - Convex Hull Algorithms
     - Polygons
     - Cell Complexes and Polyhedra
     - Arrangements
     - Triangulations and Delaunay Triangulations  
     - Voronoi Diagrams
     - Mesh Generation
     - Shape Reconstruction
     - Spatial Searching and Sorting
     - Geometric Optimization
     - Interpolation


https://doc.cgal.org/latest/Manual/packages.html


# IMAGEN CON ALGUNAS PRIMITIVAS O GRAFICOS DE CGAL

### Como funciona CGAL???

Explicar que es un kernel, objects and predicates! algorithms

https://doc.cgal.org/latest/Manual/manual.html#title1

Existen: CONCEPTOS y MODELOS y como hay clases que pueden heredar de otras, los conceptos pueden refinar otros conceptos heredando de ellas.

Una clase es modelo de un concepto

The CGAL library is a library of class templates. Consequently, we express the requirements on template arguments by specifying concepts and by providing models for concepts. See here for an explanation of concept/model.

The reference manual has pages for concepts and for models, and just as classes can be derived from other classes, concepts can refine other concepts, by adding requirements.

When a class is a model of a concept, its reference manual page has a link to the concept, and the API is mainly documented on the reference manual page of the concept. As a concept may refine another concept, the full API of a class is sometimes distributed over the pages of base classes and over the pages of several concepts.

Let's have a look at the vertex type of 3D triangulations.


At a first glance the kernel doing exact predicates and constructions seems to be the perfect choice, but performance requirements or limited memory resources make that it is not. Furthermore, for many algorithms it is irrelevant to do exact constructions. For example a surface mesh simplification algorithm that iteratively contracts an edge by collapsing it to the midpoint of the edge.

Most CGAL packages explain which kind of kernel they should use or support.
 
# Three Points and One Segment
# The Convex Hull of a Sequence of Points
# About Kernels and Traits Classes
# Concepts and Models


{% highlight cpp %}

{% endhighlight %}
