import base64
import zlib
import re

def decode_stuff(encoded_data, output_file="doomrat_decoded.txt"):
    last_iteration = ""
    iteration_count = 0
    
    while True:
        try:
            decoded_data = base64.b64decode(encoded_data[::-1])
            decompressed_data = zlib.decompress(decoded_data).decode()
        except (zlib.error, base64.binascii.Error):
            print("No more valid encoded data found or decoding failed.")
            break
        
        # Defang anyway
        decompressed_data = decompressed_data.replace("exec", "print")
        
        last_iteration = decompressed_data
        iteration_count += 1
        
        print(f"\nDecoded and modified data (Iteration {iteration_count}):\n")
        print(decompressed_data)
        
        encoded_data_match = re.search(r'b\'([A-Za-z0-9+/=]+)\'', decompressed_data)
        
        if encoded_data_match:
            encoded_data = encoded_data_match.group(1)
        else:
            print("No further encoded data found.")
            break
    
    with open(output_file, "w", encoding="utf-8") as file:
        file.write(last_iteration)
    
    # Report the number of iterations
    print(f"\nLast iteration saved to '{output_file}'.")
    print(f"Total iterations performed: {iteration_count}")


# Example base64 encoded data with reversed string
encoded_string = b'GET_THE_B64_STRING_FROM_RAT'


decode_stuff(encoded_string)
