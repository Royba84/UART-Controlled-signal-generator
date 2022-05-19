#define pi 3.14
#define signal_len 320
#define counts_per_period 64
#define input_len 10
#define MAX_VAL 15
#define MIN_VAL 0
#define ONE_V_VAL  3


char Signal[signal_len] = {0}; //signal buffer
int F = 10;                          //frequency in Hz
float T = 1/(float)F;                       //Period in sec
int A = 5;                           //amplitude
float B = 0;//10;//0;                //Amplitude of Pseudo-random flat noise
int C = 0;                               //Number of positive bumps per period T with amplitude 1 V.
int bump_idx;                          //input from serial monitor
char cur_mode = 1;
int j = 0;


void set_timer_1hz();     //64hz TIMER_1 setup,  
void set_timer_10hz();    //640hz TIMER_1 setup, 10hz period
void set_timer_50hz();    //3200hz TIMER_1 setup, 50hz period
void set_timer();

char get_input();         //get input from user
float get_num_input();  
void get_menu();         //print menu
void function_options(char val);  //select function

void sin_wave();        
void saw_wave();
void dump_cos_wave();
void set_wave();
void set_amp();
void set_period();
void set_freq();
void set_B_noise();
void set_bumps();
float calc_noise();
int calc_bumps(int idx);
void print_info();



char chk_sig(float val);





void setup() {
  Serial.begin(115200); // initalize the serial connection
  DDRB = B00001111;  // sets Arduino pins 8 to 11 as outputs
  randomSeed(analogRead(0));
  set_wave();//SETS SIN 
  
}

ISR(TIMER1_COMPA_vect) {
  if (j > (signal_len - 1))
    j = 0;
  PORTB = (Signal[j++]);
}

void loop() {
  function_options(get_input());

}


void get_menu()
{
  Serial.println("************************** Signal Generator **************************");
  Serial.println(" Please select an option");
  Serial.println("1 ---> Sin wave");
  Serial.println("2 ---> Sawtooth wave");
  Serial.println("3 ---> Dumped cosine wave");
  Serial.println("a ---> change amplitude");
  Serial.println("t ---> change period time");
  Serial.println("f ---> change frequency");
  Serial.println("b ---> adding Pseudo-random flat noise (0-0.5[v])");
  Serial.println("c ---> adding positive bumps per period (with amplitude 1[v]) in range of(0-5)");
}

void print_info()
{
	Serial.println("__________________________________________________");
  	switch (cur_mode)
    {
      case '1':
      	Serial.println("			SIN WAVE");
      	break;
      
      case '2':
      	Serial.println("			SAWTOOTH WAVE");
      	break;
      
      case '3':
      	Serial.println("			DUMPED COSINE WAVE");
      	break;
      
      default:
        break;     
    }
  	Serial.print("Amplitude: ");
  	Serial.print(A);
  	Serial.println(" [v]");
  	Serial.print("Frequency: ");
    Serial.print(F);
  	Serial.println(" [Hz]");
  	Serial.print("Time period: ");
    Serial.print(T);
  	Serial.println(" [sec]");
  	Serial.print("Pseudo-random flat noise: ");
    Serial.print(B);
  	Serial.println(" [v]");
  	Serial.print("Positive bumps per period: ");
  	Serial.println(C);
    Serial.println("__________________________________________________");
}


void set_timer_1hz()
{
  // TIMER 1 for interrupt frequency 64 Hz:
  cli(); // stop interrupts
  TCCR1A = 0; // set entire TCCR1A register to 0
  TCCR1B = 0; // same for TCCR1B
  TCNT1  = 0; // initialize counter value to 0
  // set compare match register for 64 Hz increments
  OCR1A = 31249; // = 16000000 / (8 * 64) - 1 (must be <65536)
  // turn on CTC mode
  TCCR1B |= (1 << WGM12);
  // Set CS12, CS11 and CS10 bits for 8 prescaler
  TCCR1B |= (0 << CS12) | (1 << CS11) | (0 << CS10);
  // enable timer compare interrupt
  TIMSK1 |= (1 << OCIE1A);
  sei(); // allow interrupts

}

void set_timer_10hz()
{
  // TIMER 1 for interrupt frequency 640 Hz:
  cli(); // stop interrupts
  TCCR1A = 0; // set entire TCCR1A register to 0
  TCCR1B = 0; // same for TCCR1B
  TCNT1  = 0; // initialize counter value to 0
  // set compare match register for 640 Hz increments
  OCR1A = 24999; // = 16000000 / (1 * 640) - 1 (must be <65536)
  // turn on CTC mode
  TCCR1B |= (1 << WGM12);
  // Set CS12, CS11 and CS10 bits for 1 prescaler
  TCCR1B |= (0 << CS12) | (0 << CS11) | (1 << CS10);
  // enable timer compare interrupt
  TIMSK1 |= (1 << OCIE1A);
  sei(); // allow interrupts


}

void set_timer_50hz()
{
  // TIMER 1 for interrupt frequency 3200 Hz:
  cli(); // stop interrupts
  TCCR1A = 0; // set entire TCCR1A register to 0
  TCCR1B = 0; // same for TCCR1B
  TCNT1  = 0; // initialize counter value to 0
  // set compare match register for 3200 Hz increments
  OCR1A = 4999; // = 16000000 / (1 * 3200) - 1 (must be <65536)
  // turn on CTC mode
  TCCR1B |= (1 << WGM12);
  // Set CS12, CS11 and CS10 bits for 1 prescaler
  TCCR1B |= (0 << CS12) | (0 << CS11) | (1 << CS10);
  // enable timer compare interrupt
  TIMSK1 |= (1 << OCIE1A);
  sei(); // allow interrupts

}


void set_timer()
{
   switch(F)
  {
    case 1:
      set_timer_1hz();
      break;

    case 10:
      set_timer_10hz();
      break;

    case 50:
      set_timer_50hz();
      break;

    default:
      break;     
  }
}


char get_input()
{
  char value[input_len] = {0};
  int i = 0;
  while (!Serial.available()); //wait for input
  while (Serial.available() > 0)
  { 
    value[i++] = Serial.read(); // read 1 character from serial monitor, store it in variable c
  }
  return value[0];
}


void function_options(char val)
{
  switch (val)
  {
    case '1':
      cur_mode = '1';
      sin_wave();
      break;

    case '2':
      cur_mode = '2';
      saw_wave();
      break;

    case '3':
      cur_mode = '3';
      dump_cos_wave();
      break;


    case 'a':
      set_amp();
      break;

    case 't':
      set_period();
      break;
      
    case 'f':
      set_freq();
      break;
      

    case 'b':
      set_B_noise();
      break;

    case 'c':
      set_bumps();
      break;

    
    default:
      get_menu();
      //functoin_option(get_input());  //TODO
      break;
  }

}


void sin_wave()
{
  cli(); // stop any interrupts
  int n = 0;
  float x;
  float s_n;
  
  for (int i = 0; i < counts_per_period; i++)
  {
    x = (float)i/counts_per_period;
    s_n = (1 + sin(2.0 * pi *x))*1.6* A+ calc_noise() + calc_bumps(i);
    s_n = chk_sig(s_n);
    Signal[n] = s_n;
    Signal[n + counts_per_period * 1] = s_n;
    Signal[n + counts_per_period * 2] = s_n;
    Signal[n + counts_per_period * 3] = s_n;
    Signal[n + counts_per_period * 4] = s_n;
    n++;
    //Serial.println("");
  }
 print_info();
 set_timer(); 
   
 
}


void saw_wave()
{
  cli(); // stop any interrupts
  int n=0;
  float x;
  float sw_n;
  for (int i = 0;i < MAX_VAL +1 ;i++)
  {
    for(int k = 0; k < 4 ; k++)
    {
      sw_n = i* (A/5.0) + calc_noise() + calc_bumps(i); 
      
      sw_n = chk_sig(sw_n);
      //Serial.println(sw_n);
      Signal[n] = sw_n;
      Signal[n + counts_per_period * 1] = sw_n;
      Signal[n + counts_per_period * 2] = sw_n;
      Signal[n + counts_per_period * 3] = sw_n;
      Signal[n + counts_per_period * 4] = sw_n;
      //Serial.println("");  //tinkercard error
      n++;
    }
  }
  print_info();
  set_timer();
}

void dump_cos_wave()
{
  cli(); // stop any interrupts
  int n = 0;
  float x;
  float s_n;
  for (int i = 0; i < signal_len; i++)
  {
    x =(float)i/counts_per_period;
    s_n = A *1.6* exp(-x /3.0 ) * (1 + cos( 2.0 * pi * x))+ calc_noise() + calc_bumps(i);
    Signal[n] = chk_sig(s_n);
    //Serial.println(s_n);
    n++;
  }
 print_info();
 set_timer();  
}

void set_wave()
{
  switch(cur_mode)
    {
      case '1':
        sin_wave();
        break;
      
      case '2':
        saw_wave();
        break;
      
      case '3':
        dump_cos_wave();
        break;
      
      default:
        sin_wave();
        break;
    }
      
}

void set_amp()
{
  Serial.println("please set amplitude 0-5[v]");
  float tmp = get_num_input();
  if (tmp < 0 || tmp > 5)
    Serial.println("amplitude can be 0-5[V]");
  else
    A = tmp;
  set_wave();
}

void set_period()
{
  
  Serial.println(".....Please select the signal period: 1sec/0.1sec/0.02sec.....");
  Serial.println("1 ---> 1sec");
  Serial.println("2 ---> 0.1sec");
  Serial.println("3 ---> 0.02sec");
  
  char period = get_input();
  Serial.println(period);
  
  if(period == '1')
  {
    T = 1;
    F = 1;
    set_wave();  
  } 
  else if(period == '2')
  {
    T = 0.1;
    F = 10;
    set_wave();  
  } 
  else if(period == '3')
  {
    T = 0.02;
    F = 50;
    set_wave();  
  } 
  else
  {
    Serial.println("Period llegale values are: 1sec/0.1sec/0.02sec");
  }
  
}

 
void set_freq()
{
  
  Serial.println(".....Please select the signal Frequency: 1Hz/10Hz/50Hz.....");
  Serial.println("1 ---> 1Hz");
  Serial.println("2 ---> 10Hz");
  Serial.println("3 ---> 50Hz");
  
  char period = get_input();
  Serial.println(period);
  
  if(period == '1')
  {
    T = 1;
    F = 1;
    set_wave();  
  } 
  else if(period == '2')
  {
    T = 0.1;
    F = 10;
    set_wave();  
  } 
  else if(period == '3')
  {
    T = 0.02;
    F = 50;
    set_wave();  
  } 
  else
  {
    Serial.println("Frequency llegale values are: 1Hz/10Hz/50Hz");
  }
  
}
  
void set_B_noise() //Changed it from void to float..
{
  Serial.println("Please enter a value between 0 to 0.5");
  float value = get_num_input();
  if(value >= 0.0 && value <= 0.5)
  {
    B = value;
    Serial.println(B);
    set_wave();  
  }
  else
  {
    Serial.println("Invalid input");
    B=0;
  }
}

void set_bumps()
{
  Serial.println("Please set num bumps in the range of 0-5");
  int tmp = get_num_input();
  if (tmp < 0 || tmp > 5)
      Serial.println("bumps must be between 0-5");
  else
  {
    C = tmp;
    bump_idx = (counts_per_period/C); 
    set_wave();  
  }

}

int calc_bumps(int idx)
{
  if(C)
  {
  if((idx + 1)% bump_idx == 0)
      return ONE_V_VAL;
  }
  return 0;
}
  
char chk_sig(float val)
{
  if (val > MAX_VAL)
    return MAX_VAL;
  else if (val < MIN_VAL)
    return MIN_VAL;
  else
    return (char)val;
}

      
float get_num_input()
{
  char c[10] = {0};
  int i = 0 , k = 0;
  float tmp = 0;
  while (!Serial.available());
  while (Serial.available() > 0 )
  {
    c[i++] = Serial.read();
    delay(5);
  }
  while (c[k] != 0)
  {
    if (c[k] <= '9' && c[k] >= '0')
    {
      tmp *= 10;
      tmp += (c[k] - '0');
      k++;
    }
    else if (c[k] == '.')
    {
      i = k;
      k++;
      while (c[k] != 0)
      {
        if (c[k] <= '9' && c[k] >= '0')
        {
          float g = c[k] - '0';
          g /= pow(10, k - i);
          tmp += g;
          k++;
        }
        else
        {
          Serial.println("only numbers!");
          return 0;
        }
      }
      return tmp;
    }
    else
    {
      Serial.println("only numbers!");
      return 0;
    }

  }
  return tmp;
}


float calc_noise()
{
  float temp;
  if(B!=0)
  {
    temp = B*4;
    temp = random(temp + 1);
  }
  else 
    temp = 0.0;
  //Serial.println(temp);
  return temp;
}
