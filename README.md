"""
Sanyerlis Camacaro - CCSC285 - Sancamac@uat.edu Assignment:
Assignment 2.1: Sound Enhanced Image Viewer

"InstaFolioTunes"

This code demonstrates how to:

Display a Single Image.
Create a Pygame window that is the same size as the image.
Load an image and draw it on the Pygame window.
Creating an Image List.
Write a function that gets the names of all images in a directory. Make sure to include error handling for situations where the directory doesn't exist
or doesn't contain any images.
Create a list of Pygame images by loading each image file in the directory.
Creating the Viewer: Now let's make it possible to view all the images.
Create a Pygame loop that displays each image in the list for a set amount of time (like a slideshow) AND / OR
Make sure to include controls for manually switching between images. Use Pygame's event system to listen for key presses (such as the arrow keys) 
to move to the next or previous image.
(Optional) Enhancing the Viewer: Let's add some more features to our viewer. Display the name of the current image on the Pygame window. 
You might want to look into Pygame's font module for this.
Start by displaying and moving through all four images.
Make a great UX.
Overcomment your code. 
Load a sound file and play it when the first image is displayed. 
Repeat this sound via a loop.
"""
# ************************Libraries*****************************
# Add our libraries
# Pygame library to allow us to make a GUI
import pygame
# OS library to allow us to interact with the operating system
import os

# ************************Pygame Object*****************************
# Initialize pygame - Create an instance of Pygame
pygame.init()

# ************************Window Init*****************************
# Create constants for the screen width and height
WINDOW_WIDTH = 960
WINDOW_HEIGHT = 1000
# Create a window object
window = pygame.display.set_mode((WINDOW_WIDTH, WINDOW_HEIGHT))
# Set the title of the window
pygame.display.set_caption("InstaFolio")

# *************************Files****************************
# Set the path to our images folder, this will be a constant
IMAGE_DIRECTORY = "images"
# Get a list of all the files in the images folder
image_filenames = [filename for filename in os.listdir(IMAGE_DIRECTORY) if filename.endswith(".jpeg") or filename.endswith(".PNG") or filename.endswith(".gif") or filename.endswith(".jpeg") or filename.endswith("jfif")]

# ****************** Load each image into a list we can loop through **********************
# Create an empty list for our images
images = []
# Loop through each image filename
for filename in image_filenames:
    # Get the full path to the image
    image_path = os.path.join(IMAGE_DIRECTORY, filename)
    # Load the image into memory
    image = pygame.image.load(image_path)
    # Add the image to our list of images
    images.append(image)

# ************************* Font Setup ****************************
# Import the font module
import pygame.font

# Create a font object
font = pygame.font.Font(None, 36)  # You can adjust the font size as needed

# ************************* Sound Setup ****************************
# Import the sound mixer module
import pygame.mixer

# Create a function to play our mp3 sound files
def play_mp3(mp3_filename):
    # Set the audio properties
    pygame.mixer.init(frequency=44100, size=-16, channels=2)

    # Load the mp3 file
    pygame.mixer.music.load(mp3_filename)

    # Play the mp3 file
    pygame.mixer.music.play()

# Set the mp3 filename
mp3_filename = "audio/Spaceship.mp3"
mp3_name = os.path.basename(mp3_filename)  # Get the name of the audio file

# Play the audio when the first image is displayed
play_mp3(mp3_filename)

# ******************************** Main Program Loop - Display Images *******************************************
# Create a variable to hold the index of the current image
current_image_index = 0
# Create a variable to hold the current image from the image list
current_image = images[current_image_index]
# Create a clock object to control the frame rate
clock = pygame.time.Clock()
# Create and start our main program loop
# This bool will keep the window open until we set it to false
is_running = True
# Loop until running is set to false
while is_running:
    # Clear the screen
    window.fill((255, 255, 255))
    
    # Display the current image
    window.blit(current_image, (0, 0))
    
    # Render the image name as text
    image_name = image_filenames[current_image_index]
    text = font.render(image_name, True, (0, 0, 0))  # Text color: black
    
    # Render the audio name as text
    audio_text = font.render("Audio: " + mp3_name, True, (0, 0, 0))  # Text color: black
    
    # Render the message in the center under the audio name
    message_text = font.render("Can you escape the Matrix?", True, (0, 0, 0))  # Text color: black
    message_rect = message_text.get_rect(center=(WINDOW_WIDTH // 2, (WINDOW_HEIGHT // 2) + 50))

    # Blit the text onto the window
    window.blit(text, (10, 10))  # Adjust the position as needed
    window.blit(audio_text, (10, 50))  # Adjust the position as needed
    window.blit(message_text, message_rect)

    # Update the display
    pygame.display.flip()
    
    # Handle any events
    for event in pygame.event.get():
        if event.type == pygame.KEYDOWN:
            if event.key == pygame.K_RIGHT:
                current_image_index = (current_image_index + 1) % len(images)
                current_image = images[current_image_index]
                # Set the new audio name
                mp3_name = os.path.basename(mp3_filename)  # Get the name of the audio file
                play_mp3(mp3_filename)
            elif event.key == pygame.K_LEFT:
                current_image_index = (current_image_index - 1) % len(images)
                current_image = images[current_image_index]
                # Set the new audio name
                mp3_name = os.path.basename(mp3_filename)  # Get the name of the audio file
                play_mp3(mp3_filename)
            elif event.key == pygame.K_ESCAPE:
                is_running = False

    # Set the frame rate to 30 frames per second
    clock.tick(30)

# ******************************* End of program ******************************
# Clean up the pygame session
pygame.quit()
