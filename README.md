# Stall-Occupier
displays stalls and computes random occupation 


void setup() {
  size(400, 400);
  // creates parking stall objects
  float xx = 5;
  float yy = 5;
  float stallwidth = 38;
  float height = 70;
  ParkingStall [] [] parkingstall_array = new ParkingStall [3][10];
  for (int i = 0; i< parkingstall_array.length; i++) {
    for (int j = 0; j<parkingstall_array[i].length; j++) {
      int determinant = (int)(random(0, 2));
      parkingstall_array[i][j] = new ParkingStall(xx, yy, stallwidth, height, determinant);
      parkingstall_array[i][j].drawStall();
      // sets parking stalls time of occupation
      Date ps1_date = new Date (3, 7, (int)(random(0, 60)), true);
      parkingstall_array[i][j].setStatus(determinant, ps1_date);
      xx += stallwidth;
      parkingstall_array[i][j].print_timeTaken(i+1, j+1);

      // displays the time stall occupied on the console
    }
    yy+= 130;
    xx= 5;
  }
}



    class ParkingStall {
      // STALL ATTRIBUTES
      boolean occupied;
      Date timeTaken;

      // DIMENSIONS AND POSITION
      float stallWidth;
      float stallHeight;
      float posX;
      float posY;

      ParkingStall(float x, float y, float w, float h, int determinant) {
        if(determinant == 0)
          occupied = false;
         else
         occupied = true;
        posX = x;
        posY = y;
        stallWidth = w;
        stallHeight = h;
      }

      void drawStall() {
        if (occupied)
          fill(color(255, 90, 71)); // RED STALL
        else
          fill(color(152, 251, 152));  // GREEN STALL
        strokeWeight(4);
        stroke(255);
        rect(posX, posY, stallWidth, stallHeight);
        strokeWeight(1);
      }

      // Sets whether the stall is occupied or not
      void setStatus(int status, Date time)
      {
        if (status == 1) {
          occupied = true;
        }
        if (status == 0) {
          occupied = false;
        }
        if (occupied)
          timeTaken = new Date(time);
      }

      void print_timeTaken(int row, int column) {
        if (occupied)
          println("stall " + row + ", " + column + " was occupied at: " + timeTaken);
      }
    }
    
    
    
    Date Class
    
    class Date {
  final String [] days = {"Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday", "Sunday"};
  int today; 
  int hour;
  int minute;
  boolean before_noon;
  Date(int d, int h, int m, boolean beforeNoon) {
    if (d>6)
      d = d%7;
    if (h>12)
      h = h%12;
    if (m>59)
      m = m%60;
    today = d;
    hour = h;
    minute = m;
    before_noon = beforeNoon;
  }

  Date(Date mydate) {
    today = mydate.today;
    hour = mydate.hour;
    minute = mydate.minute;
    before_noon =  mydate.before_noon;
  }


  void addHour() {
    hour++;
    if (hour>12)
      hour = hour%12;
  }

  void addMinute() {
    minute++;
    if (minute>59) {
      minute = minute%59;
      addHour();
    }
  }

  boolean equal(Date date) {
    if (today == date.today && hour == date.hour && minute == date.minute && before_noon == date.before_noon)
      return true;
    else
      return false;
  }
  String toString() {
    String date = days[today];
    if (hour <= 10)
      date += " 0" + hour;
    else
      date += " " + hour;

    if (minute  <= 10)
      date+= ":0" + minute;
    else
      date+= ":" + minute;

    if (before_noon)
      date+= "AM ";
    else 
    date+= "PM ";
    return date;
  }
}
