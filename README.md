//// Project 2
//// (Assume ball diameter of 50=Z.)

//// GLOBALS:  pool table, 4 colored balls

String title=  "ELASTIC COLLISIONS  (x5.java)";
String news=   "Use 'r' key  or click cue ball to reset.";

String author=  "Brian Salaway";


float left, right, top, bottom;
float middle;

float cueX,  cueY,  cueDX,  cueDY;
float redX,  redY,  redDX,  redDY;
float yelX,  yelY,  yelDX,  yelDY;
float bluX, bluY, bluDX, bluDY;
float button1X=left+50, button1H=top+25, button1W=left+100, button1Y=top+50;////reset 


/////////////// Diameter of balls button
int Z=50;
int V=51;

//// SETUP:  size and table
void setup() {
  size( 600, 400 );
  left=   50;
  right=  width-50;
  top=    100;
  bottom= height-50;
  middle= left + (right-left) / 2;
  
  reset();
 }
 
 void reset() {
   cueX=  left + (right-left) / 4;
   cueY=  top + (bottom-top) / 2;
   
    /////////// Rack 'Em Up!
  redX= width*2/3; redY= (top + bottom)/2; 
  yelX= redX + 55; yelY = redY +45;
  bluX= redX +55;  bluY = redY-45;
  
  
   //  speeds starting at rest
   redDX=  random( 0 );   redDY=  random( 0 );
   yelDX=  random( 0);   redDY=  random(0 );
   bluDX=  random( 0 );   bluDY=  random( 0 );
 }
 
 //// NEXT FRAME:  table, bounce off walls, collisions, show all
void draw() {
  background( 250,250,200 );
  rectMode( CORNERS );
  table( left, top, right, bottom );
  bounce();
  collisions();
  show();
  messages();
}

//// SCENE:  draw the table with walls
void table( float left, float top, float right, float bottom ) {
  fill( 100, 250, 100 );    // green pool table
  strokeWeight(20);
  stroke( 127, 0, 0 );      // Brown walls
  rect( left-20, top-20, right+20, bottom+20 );
  stroke(0);
  strokeWeight(1);
   fill(192, 192, 192);
  rect(button1X, button1Y, button1W, button1H); /////reset button
  fill(0);
  text("reset", button1X+10, button1Y-10);
 
}

//// ACTION:  bounce off walls, collisions
void bounce() {
  
  cueX +=cueDX; if (cueX<left ||cueX>right) cueDX *= -1;
  cueY +=cueDY; if (cueY<top ||cueY>bottom) cueDY *= -1;
  
  redX += redDX;  if ( redX<left || redX>right ) redDX *= -1;
  redY += redDY;  if ( redY<top || redY>bottom ) redDY *=  -1;

  yelX += yelDX;  if ( yelX<left || yelX>right ) yelDX *= -1;
  yelY += yelDY;  if ( yelY<top || yelY>bottom ) yelDY *=  -1;
  
  bluX += bluDX;  if ( bluX<left || bluX>right ) bluDX *= -1;
  bluY += bluDY;  if ( bluY<top || bluY>bottom ) bluDY *=  -1;

}

void collisions() {
  float tmp;
  // Swap velocities!
  if ( dist( redX,redY, yelX,yelY ) < Z ) {
    tmp=yelDX;  yelDX=redDX;  redDX=tmp;
    tmp=yelDY;  yelDY=redDY;  redDY=tmp;
  }
  if ( dist( redX,redY, bluX,bluY ) < Z ) {
    tmp=bluDX;  bluDX=redDX;  redDX=tmp;
    tmp=bluDY;  bluDY=redDY;  redDY=tmp;
  }
 if ( dist( bluX,bluY, yelX,yelY ) < Z ) {
    tmp=yelDX;  yelDX=bluDX;  bluDX=tmp;
    tmp=yelDY;  yelDY=bluDY;  bluDY=tmp;
  } 
 if ( dist( bluX,bluY, cueX,cueY ) < Z ) {
    tmp=cueDX;  cueDX=bluDX;  bluDX=tmp;
    tmp=cueDY;  cueDY=bluDY;  bluDY=tmp;
  } 
  if ( dist( yelX,yelY, cueX,cueY ) < Z ) {
    tmp=cueDX;  cueDX=yelDX;  yelDX=tmp;
    tmp=cueDY;  cueDY=yelDY;  yelDY=tmp;
  } 
  if ( dist( redX,redY, cueX,cueY ) < Z ) {
    tmp=cueDX;  cueDX=redDX;  redDX=tmp;
    tmp=cueDY;  cueDY=redDY;  redDY=tmp;
  }
}
    
    
    
//// SHOW:  balls, messages
void show() {

  fill( 255,0,0 );    
  ellipse( redX,redY, Z,Z );  
  fill(0); 
 
   text( "2", redX,redY );
       
  fill( 255,255,0 );  
  ellipse( yelX,yelY, Z,Z );
   fill(0);
   text( "3", yelX,yelY );
  fill( 0,0,255 );    
  ellipse( bluX,bluY, Z,Z );
   fill(0);
   text( "4", bluX,bluY );
  fill( 255,255,255 );    
  ellipse( cueX,cueY, Z,Z );
   fill(0);
   text( "1", cueX,cueY );
}
void messages() {
  fill(0);
  text( title, width/3, 20 );
  text( news, width/3, 40 );
  text( author, 10, height-10 );
}



void mousePressed (){
  
  
 

  if (  mouseX >= button1X && mouseX <=  button1W 
 && mouseY >= button1H && mouseY <= button1Y){    
    reset();
}
  
   {
  ///////Break----distance=force
  float force= dist(mouseX,mouseY, abs(cueX-V),abs(cueY-V))/20;
  strokeWeight(force);
  line(mouseX,mouseY, cueX,cueY);
  strokeWeight(1);
  
  cueDX=((cueX-mouseX)/20);
  cueDY=((cueY-mouseY)/20);    
    
{ if(mouseX<=left||mouseX>=right){{ cueDX =0;} {cueDY=0;}}
   if(mouseY<=top||mouseY>=bottom){{ cueDX=0;} {cueDY=0;}}
  }
  
  {
    if (  mouseX >= cueX -Z || mouseX < cueX + Z &&  mouseY >= cueY -Z || 
  mouseY < cueY + Z) { 
    reset();  }
}
  }  
}
//// HANDLERS:  keys, click
void keyPressed() {
  if (key == 'r') {
    reset();
  }
  if(key=='q'){exit();}
}


  
 

