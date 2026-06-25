# /unpack-emma-stills

Unzip emma-image-files.zip from X:\ root and place slugged stills into the correct asset folder.

## Steps

1. Create destination if it doesn't exist:
   `mkdir -p "X:\DISMOUNT\asmr-fantasy\assets\emma-stills"`

2. Unzip:
   `Expand-Archive -Path "X:\emma-image-files.zip" -DestinationPath "X:\DISMOUNT\asmr-fantasy\assets\emma-stills" -Force`

3. Verify 5 files present:
   `ls "X:\DISMOUNT\asmr-fantasy\assets\emma-stills"`

   Expected:
   - emma-home-soft.png
   - emma-home-smile.png
   - emma-work-smile.png
   - emma-work-neutral.png
   - emma-work-essco.jpg

4. Report result to Dale.
