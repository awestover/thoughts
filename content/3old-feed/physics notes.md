**MAXWELL**
Gauss's Electric Law :
$$\oint_{\partial V} \mathbf{E} \cdot d\mathbf{A} = \frac{Q_{\text{enc}}}{\varepsilon_0}$$
Gauss's Magnetic Law :
$$
\oint_{\partial V} \mathbf{B}\cdot d\mathbf{A} = 0
$$
Faraday Induction Law:
$$\oint_{\partial S} \mathbf{E} \cdot d\mathbf{l} = -\frac{d}{dt} \Phi_B$$
    
Ampère's Law (with Maxwell's correction)
$$\oint_{\partial S} \mathbf{B} \cdot d\mathbf{l} = \mu_0 \left( I_{\text{enc}} + \varepsilon_0 \frac{d}{dt} \Phi_E \right)$$

**inductance**
$L=\mu N^{2} A/l$

$U=\frac{1}{2}CV^{2}$

$u_E = \frac{1}{2}\varepsilon_0 E^{2}$

$u_B=\frac{B^{2}}{2\mu_{0}}$

$L=\Phi/I$

$C=Q/V$

$L=2U_B/I^{2}$.

$u_B = \frac{1}{2\mu_{0}}B^{2}$

$U_B  = \frac{1}{2}LI^{2}.$

**LR circuits**

voltage drop across an inductor: $L I'$

**ENM -- DIPOLES:**

$\rho=\sum r_i q_i$
$\mu = IA \hat{n}$

$$
U = - \rho \cdot E
$$
$$
\tau=\rho \times E
$$
$$
F=-\nabla U 
$$
$$
U=-\mu \cdot B
$$
$$
\tau=\mu \times B
$$


**Gauss's Law** 
Note that this is not actually a law, it's just a corollary of Coulomb's Law.
Electric flux through a closed surface is equal to enclosed charge over $\varepsilon_{0}$.
$$
\Phi_E = \iint_A (\vec{E} \cdot \hat{n}) dA = Q/\varepsilon_{0} = 4 \pi k Q.
$$

- Electric field lines tell you where a positive test charge would go
- Dipole moment points from negative charge to positive charge
	- this makes sense, because the general principle is that we give priority to positive dudes and let negative dudes flip signs. 


**slit experiments**

**Double slit**
ok we're going to make an approximation: 
$L\gg d$, so we can assume that the angle of the ray from either slit to some point on the wall is roughly the same. Fine. Let's fix some point $P$ on the wall and suppose that rays from the slits going to point $P$ have angle (above the horizontal) of approximately $\theta$.

Then, if you wanted one of the waves to travel an extra wave-length versus the other, you'd get constructive interference whenever $\theta$ satisfies
$$
d\sin \theta = \mathbb{Z}\lambda
$$
And the separation between peaks is 
$$
L\sin \theta  = \frac{\lambda L}{d}.
$$
You'd get destructive interference when $\theta$ satisfies
$$
d\sin \theta = (\mathbb{Z}+1/2)\lambda.
$$


**Single slit**
MINIMA at $a\sin \theta = \mathbb{Z}\lambda$ where $a$  is width of slit. 

**BOTH**  i.e. you have two slits but they aren't negligibly thin. 
Then you just combine both cases; good luck.


**Stefan Boltzman Law** -- Hot stuff emits EM waves.
$$
\text{intensity} = P/A = \sigma T^{4}.
$$

Note: 

Time average of Ponyting Vector gives intensity of EM wave!!


Note poynting vector 
$$
S = \frac{1}{\mu_{0}}E \times B
$$
**Radiation pressure:** time avg of ponyting vector OVER c
Prad = S/c

**ENERGY density**
$$
u = \frac{\varepsilon_{0}}{2}E^2 + \frac{1}{2\mu_{0}}B^2 
$$
In vacuum these two terms are the same. 

$S = uc$

Ah so radiation pressure is the same as energy density -- weird.