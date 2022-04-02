Mathematical description of a means to determine if and when
a three dimensional line intersects a three dimensional thing
-------------------------------------------------------------

Written in the achaic language of English, the following describes the means
by which a very important part of collission algorithms are carried out in
virtual reality worlds.

When two balls bounce off each other in a virtual reality world, there must
at some level be an ability to determine if the balls collide or not. In the
prose beneath, a ball is described as "a thing" as it can be a spaceship or
an arm of a non-player character bouncing off a virtual table.

The long and the short of it is that these "things" are made up of faces
which are made up of vertices and at some level, one must be able to work
out whether a line
      from one point (say a vertice on a face of a ball) to
      another (where that vertice will be when the ball moves)
intersects another thing.

This "projecy" is just a description of how that process can be carried
out for "things" with faces of any number of vertices.

There are efficiencies which can be discerned when dealing exclusively
with faces which are limited to three vertices (triangles).

		A line from position pos, with a unit vector vec can be described by the
		equation:
						Ax+By+Cz-D=0

							where A is vec[0], B is vec[1] and C is vec[2]
							and D is vec[0]*pos[0]+vec[1]*pos[1]+vec[2]*pos[2].

		A line Ax+By+Cz=D intersects a thing if it intersects any of its faces.
		
		Each face can be described to have (at least) three points K, L, and M.

		The normal to a face N is the cross product of two vectors between these
		points (For example K->L and L->M), can be described by the equation:

						Px+Qy+Rz=0

							where P is N[0], Q is N[1] and R is N[2].

		The plane which N is normal to can be described by the equation:

						Px+Qy+Rz-S=0
						
							where S can be deduced from any of the points K,L or M;
							for example S=P*K[0]+Q*K[1]+R*K[2].

		A line Ax+By+Cz-D=0 intersects a plane Px+Qy+Rz-S=0 if and when:

					Ax+By+Cz-D = Px+Qy+Rz-S
						
		which is to say, if and when:
						
					Ax-Px+By-Qy+Cz-Rz-D+S=0

		which is also to say, if and when

					(A-P)x+(B-Q)y+(C-R)z-D+S=0

		This equation may be satisfied by a point on the line Ax+By+Cz=D
		which can be described by 

					(A*t,B*t,C*t)
		
						where t can be deduced from this point being on the plane

							Px+Qy+Rz-S=0
						
						such:
							
							P(A*t)+Q(B*t)+R(C*t)-S=0
							PAPt+QBQt+RCRt-S=0
							t(PAP+QBQ+RCR)-S=0
							t(PAP+QBQ+RCR)=S
							t=S/(PAP+QBQ+RCR)

					The point does not exist if (PAP+QBQ+RCR) is zero. This is the
					situation which occurs if the line is parallel to the plane.

					In the circumstance where the point does exist, its co-ordinates
					are determinned as:

						(A*(S/PAP*QBQ+RCR),B*(S/PAP*QBQ+RCR),C*(S/PAP*QBQ+RCR))
						
		A line intersects a face if it intersects the plane that the face is on
		and the point at which the line intersects the plane is inside the hull
		of points for the face.
		
		Looking at the face from any orthographic perspective will produce a two
		dimensional depiction of the face and its vertices and the intersection
		point we have already deduced.
		
				Given that epsilon is the smallest number	that can be represented by a
				floating point value on a digital computer, the ideal orthographic
				perspective would be in the direction of the planes normal such that the
				vertices and the intersection point are least subject to error.
		
				Given the limitations of computing in deeply nested procedures, we can
				forgo the need carry out an orthographic rotation, by simply looking at
				the face on either the XY, XZ or ZY plane, but ideally on the plane which
				projects the face as "least flat". As the number of vertices increases, it
				may be desirable computationally, to carry out an orthographic projection.
		
				A face is "least flat" looking on the biggest plane of a bounding box. So
				for example:
	
					Given the points (0,0,0), (1,3,1) and (5,1,1), the bounding box
					could be described as having [5,3,1] dimensions. The orthographic
					plane which would be "least flat" would, in this case be the XY
					plane.
		
				Looking at the face projected orthographically by some means
				described above, we can ignore one the subcoordinates and proceed
				to test if a point is within the vertices of the face or not.
		
		To determine if a point is within the hull of vertices for a complex face
		containing many vertices, we create a bound line between the intersection
		point	and a point which is guaranteed to be outside the hull. A good
		example would be an arbitrary point lying outside of the far clipping
		distance of the projection frustum or a point lying outside the map of the
		world being rendered.
		
		This intersection bound line will be used to test each of the bound lines
		described by the polygon of vertices of the face. If the the intersection
		bound line intersects an even number of bound lines in that polygon then
		the intersection point is outside. (This includes the case of zero
		intersections.) The intersection point is inside the hull if there are an
		odd number of intersections.
		
			Two bound lines intesect if there is an intersection point and it is is
			within the bounding points of those bound lines.
		
			Two lines Ex-Fy+G=0 and Hx-Iy+J=0 can be written as

							y=(E/F)x+(G/F) and y=(H/I)x+(J/I)
						
						the lines intersect when

							(E/F)x+(G/F)  =  (H/I)x+(J/I)
							((E/F)-(H/I)) x  = ((J/I)-(G/F))
							x  = ((J/I)-(G/F)) / ((E/F)-(H/I))
							
						knowing the x co-ordinate of the intersection allows us to
						determine the y co-ordinate from either of the two equations
						of	the lines.


