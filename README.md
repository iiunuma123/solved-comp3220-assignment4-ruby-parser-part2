Download Link: https://assignmentchef.com/product/solved-comp3220-assignment4-ruby-parser-part2
<br>
For this part of the Ruby Interpreter Collec&gt;on of Assignments we will be modifying our exis&gt;ng Parser so that instead of prin&gt;ng parser output, it will build an <strong>AST</strong> (abstract syntax tree) that our interpreter can use to “run” programs wriHen in the Tiny Language.

<h1>Details</h1>

I have given you all the files necessary to complete the assignment. You will need to modify <strong>Parser.rb</strong> so that it builds the AST while it is parsing a file.

I have included an <strong>AST.rb</strong> file that defines an AST node object that can be used to build this tree. It also has a method that can print the tree in list format (convenient since our interpreter will be wriHen in a list-based language!).

<strong>You should NOT modify AST.rb unless you are a&gt;<a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="56333b26163831">[email protected]</a> the extra credit (see Extra Credit <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="c8bbadab88a7a6">[email protected]</a> below). </strong>

I have done some of the work for you in Parser, so that you can use that as an example of how you should proceed. At the end of this document are screenshots showing what correct output should look like given some input files that I will also provide to you for debugging.

<strong>Note</strong>: We are building an <strong>AST</strong> and <strong>NOT</strong> a parse tree. What is the difference anyway!? The difference is that a <strong>Parse Tree  </strong>

parse tree would look much like the examples we went through in class when we compared parse trees to deriva&gt;ons. They would INCLUDE every rule in our class as root nodes or sub nodes in the tree. However, an  <strong>AST (Abstract Syntax Tree) </strong>

AST <strong>ONLY</strong> includes the actual lexemes that are needed to be interpreted. If you take a look at the compiler series ar&gt;cle here: <a href="https://ruslanspivak.com/lsbasi-part1/">hHps://ruslanspivak.com/lsbasi-part1/</a> , they illustrate that in one of the series ar&gt;cles. Here is the specific ar&gt;cle in the series that introduces AST’s: <a href="https://ruslanspivak.com/lsbasi-part7/">hHps://ruslanspivak.com/lsbasi-part7/</a> .

For our assignment, we will always start with a program node as the root of the whole tree. That is the only “not concrete” thing that should be part of our tree.

<table width="694">

 <tbody>

  <tr>

   <td colspan="2" width="694"><strong>Extra Credit (25 pts) – must score 100 to qualify for any extra credit points. Otherwise, you only get 20 pts per test </strong></td>

  </tr>

  <tr>

   <td width="76"><strong>case passed.</strong></td>

   <td width="618"><strong>  </strong></td>

  </tr>

 </tbody>

</table>

<h1>Part 1</h1>

Because of the way our grammar is defined (and because we want to produce a tree in list format) our tree <strong>WILL</strong> be nested correctly <strong>BUT</strong> our operands will be in reverse order. Since it is something that is consistent, it is an easy problem to handle in our interpreter (we already know it will behave this way, so just read in the operands in reverse order).

However, it is possible to produce the tree in list form with the operands in correct order. In order to do this, you will need to modify the <strong>AST.rb</strong> class. One way to do this is to:

<ul>

 <li>add a method in <strong>rb</strong> that allows you to swap the first child of a node</li>

 <li>instead of calling <strong>addChild</strong> everywhere in <strong>rb</strong>, there are at least 2 instances where you will need to call your new custom func&gt;on instead (maybe called <strong>addAsFirstChild</strong>).</li>

</ul>

<h1>Part 2</h1>

This poten&gt;al issue is a bit harder to fix (and describe) and requires that you (1) have a really good understanding of exactly how this AST tree works and (2) how our interpreter is going to process our tree (specifically that any math operator (except for equal) requires at least 2 operands). So, (- 3 4) is ok, but (- 4) is not. Scheme can actually handle this, but our interpreter won’t be able to. See examples below.

To get a really good understanding of how our AST tree is being built, I would recommend studying the <strong>toStringList()</strong> method in AST.rb to understand how its going through the tree in order to print things. You could then create a second <strong>toStringList()</strong> in AST.rb that is a modifica&gt;on of the first one. You could aHempt to create this second one so that instead of prin&gt;ng the Token AND the Lexeme, it ONLY prints the lexeme. This will actually end up giving you something you can copy and paste into scheme to verify if it’s compu&gt;ng the answer correctly.

Example of the problem:

1 – 2 – 3 – 4 is -8.

You can type that into a calculator and get the correct result. However, you can’t type that into scheme (the way it is wriHen) and get a correct result. Since scheme is a list-based language, it expects that the first item in the list be the operator.

If you don’t aHempt the first part of the extra credit (5 pts), this is what your parser will produce. You can get this string exactly, by simply removing the Token names (or by wri&gt;ng a second <strong>toStringList()</strong> method that will print lexemes only). In this example, we are not reversing first operand (we can handle this in Interpreter). You can paste the par&gt;al tree below into scheme and it will produce a result, however, our interpreter will not be able to process it because there is an operator with just a single operand in this list; the inner <strong>( – 4 )</strong>.

( – 2 ( – 3 <strong>( – 4 ) </strong>) 1 )   ! scheme will give you -6 as a result, which is s&gt;ll incorrect, even though it runs

If you do aHempt the first part of the extra credit (5 pts), this is what your parser will produce. Note that we STILL have that inner list <strong>( – 4 )</strong> where the minus operator has only a single operand, the 4. Although you could type this expression into scheme and it would give you a correct result, our interpreter will s&gt;ll crash because of the the inner list <strong>( – 4 )</strong>.

( – 1 2 ( – 3 <strong>( – 4 )</strong> ) )

In addi&gt;on to swapping the first child of some nodes (part 1) you will also occasionally need to “reverse” an en&gt;re subtree. You can think about this as if you were “flipping” a sub-tree over, where the boHom (the leaf/leaves) become the new root and the top (the root) becomes the new leaf/leaves. Doing this would produce a tree that scheme could use to correctly calculate a result AND our interpreter will ALSO be able to use the tree to produce the correct output.

Con&gt;nuing the example above, this method would produce the following list that would be correct for our interpreter.

( – ( – ( – 1 2 ) 3 ) 4 ) ! I’ll go over this in class

<h1>Screenshots No extra credit</h1>

input1.&gt;ny




input2.&gt;ny




input3.&gt;ny




input4.&gt;ny (this one has syntax errors)




input5.&gt;ny (this one has syntax errors)

Extra credit – MUST score 100 to get the +5 points, otherwise you simply get 20 pts for each test passed. input1.&gt;ny (20 pts)




input2.&gt;ny




input3.&gt;ny




input4.&gt;ny (this one has syntax errors)




input5.&gt;ny (this one has syntax errors)

Extra credit (20 pts) – MUST score 100 AND get 5 pts extra credit to qualify for these points. MUST pass BOTH test cases below to earn the <a href="/cdn-cgi/l/email-protection" class="__cf_email__" data-cfemail="ec8d888885ac83828d80">[email protected]</a> 20 points. input6.&gt;ny




input7.&gt;ny


