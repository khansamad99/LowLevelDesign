# Movie Booking App Low-Level Design (LLD) Documentation

## Table of Contents

1. [Flow](#flow)
2. [Requirements](#requirements)
3. [Objects Identification (Classes)](#objects-identification-classes)
4. [Classes and UML Diagram](#classes-and-uml-diagram)

## Flow

1. **User selects a location/city.**
2. **User views movies available in the selected city.**
3. **User selects a movie.**
4. **Application displays theaters showing the selected movie along with showtimes.**
5. **User selects a showtime and theater.**
6. **User selects seats.**
7. **User completes booking and makes payment.**

## Requirements

- User location selection
- Display of movies based on selected city
- Theater and showtime selection
- Seat selection
- Booking and payment process
- Concurrency management for booking seats

## Objects Identification (Classes)

- **User**
- **Movie**
- **City**
- **Theater**
- **Screen**
- **Show**
- **Seat**
- **Booking**
- **Payment**

## Classes and UML Diagram

### Movie

- String `movieId`
- String `name`
- int `duration`
- Enum `Language`

### City

- String `cityId`
- String `name`
- List<Movie> `movies`

### Theater

- String `theaterId`
- String `name`
- Address `address`
- List<Screen> `screens`

### Screen

- String `screenId`
- Theater `theater`
- List<Seat> `seats`

### Show

- String `showId`
- Movie `movie`
- Screen `screen`
- DateTime `startTime`
- List<Seat> `bookedSeats`

### Seat

- String `seatId`
- Screen `screen`
- SeatType `type`
- boolean `isBooked`

### Booking

- String `bookingId`
- Show `show`
- List<Seat> `seats`
- User `user`
- Payment `payment`

### Payment

- String `paymentId`
- Booking `booking`
- double `amount`
- PaymentStatus `status`

### UML Diagram

Please find the UML Diagram linked [here](#).

### Concurrency Management

To handle concurrency in seat booking, an optimistic locking strategy is suggested. This involves maintaining a version for each seat. When a user attempts to book a seat, the system checks if the seat version has changed since it was last read. If the version is unchanged, the booking proceeds; otherwise, the booking fails, and the user is prompted to retry.

