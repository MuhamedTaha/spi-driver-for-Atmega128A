spi-driver-for-Atmega128A
=========================
/*
 * spi.c
 *
 * Created: 9/30/2014 05:12:58 AM
 *  Author: mohamed taha
 */ 

#include <avr/io.h>
#include "spi.h"
 void spi_masterinit()
 {
	 DDRB |= (1<<PB0) | (1<<PB1) |(1<<PB2);// set SCK , SS and MOSI as outputs.
	 DDRB &=~(1<<PB3); //set MIOS as input.
	 SPCR |= (1<<MSTR) |(1<<SPE) | (1<<SPR0) | (1<<SPR1); // enable master and spi mode ...set SCK =f/4.
 }
 
 void send(unsigned char data)
 {/* put data at spi data register */
	 SPDR = data;
  /*  Wait for transmission complete */ 
	 while(!(SPSR & SPIF ));
 }
 
 void spi_slaveinit()
 {
	 DDRB |= (1<<PB3); /* set MISO as output */
	 DDRB &=~ (1<<PB0);/* set ss as input */
	 DDRB &=~ (1<<PB1);/* set SCK as input */
	 DDRB &=~ (1<<PB2);/* set MOSI as input */
	 SPCR |= (1<<SPE); /* enable the spi */
 }
 
 unsigned char recive (void)
 {
	 /* Wait for reception complete */
	 while(!(SPSR & (1<<SPIF)))
	 ;
	 /* Return data register */
	 return SPDR;
 }
