# Tips for WeBWorK Authors: Gotchas!

Some things to watch out for when authoring WeBWorK problems:

-
### Perl variable with a negative value used in a (perl) formula 
For example:  Suppose you have a variable $a that could take on negative values, and you use it in a calculation of another variable. Warning: -$a (without a space between the minus sign and the Perl variable $a) can be misinterpreted by perl.   
Ways to fix possible misinterpretation:  

	$a = random(-10,10,1);  
 	$good1 = 2 - $a;     # Put a space before the variable  
	$good2 = 2 - ($a);   # Put brackets around the variable  
    $good3 = 2 -1*$a;   # Pre-multiply the variable by -1  
    $good4 = Compute("2-$a");  # Use MathObjects  
    $bad1 = 2-$a;         # What not do to
    
In summary, -$a (without a space between the minus sign and the Perl variable $a) can be misinterpreted in Perl.

-


### Uppercase U in MathObjects Compute (Formula)

For example: Suppose you want to display variables other than x, chosen randomly from a list of variables. In a MathObject context like Numeric, the uppercase U is interpreted as the union symbol. Thus, it is best to avoid the uppercase U as a variable in your list of choices.

	Context("Numeric");
	@goodvar = qw(u V W Z v w z);    # List of variables: best to AVOID uppercase U as a variables
	@badvar = qw(U);
	$x = $goodvar[random(0,$#goodvar,1)];   # Choose a variable at random
	$y = $badvar[0];
	Context()->variables->are($x=>"Real",$y=>"Real");   # Declare the new variables 
	$f = Compute("2 $x^3 + 4");      # This will work for all choices in the @goodvar array of variables
	$g = Compute("2 $y^3 + 4");    # This won't work because U is interpreted as union in MathObject
	$h = "\( 2 $y^3 + 4 \)";             # U in a LaTeX math mode string works fine.
	
-

### Beware $a\^2 in straight perl

	$a = 3;
	$b = $a^2;  # CAREFUL! Not how to do exponents in perl
	$c = $a**2; # This is the way to do exponents in perl
-
Last update: November 11, 2021
