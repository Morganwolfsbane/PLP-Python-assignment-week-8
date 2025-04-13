# PLP-Python-assignment-week-8
import os
import sys

def main():
    print("📂 File Content Transformer 📝")
    print("This program reads a file, transforms its content, and saves it to a new file.\n")
    
    while True:
        try:
            # Get file paths
            source_file = input("Enter the source file path: ").strip()
            if not source_file:
                raise ValueError("Source file path cannot be empty")
            
            dest_file = input("Enter the destination file path: ").strip()
            if not dest_file:
                raise ValueError("Destination file path cannot be empty")
            
            # Validate paths
            if os.path.exists(dest_file):
                confirm = input(f"⚠️ '{dest_file}' already exists. Overwrite? (y/n): ").lower()
                if confirm != 'y':
                    print("Operation cancelled.")
                    continue
            
            # Process file
            process_file(source_file, dest_file)
            print(f"\n✅ Success! Modified content saved to '{dest_file}'")
            break
            
        except FileNotFoundError:
            print(f"\n❌ Error: File '{source_file}' not found")
        except PermissionError:
            print("\n❌ Error: You don't have permission to access this file")
        except UnicodeDecodeError:
            print("\n❌ Error: Could not read file (possibly a binary file)")
        except ValueError as e:
            print(f"\n❌ Error: {e}")
        except Exception as e:
            print(f"\n❌ Unexpected error: {type(e).__name__} - {e}")
            sys.exit(1)
        
        # Retry logic
        retry = input("\n🔄 Would you like to try again? (y/n): ").lower()
        if retry != 'y':
            print("Goodbye!")
            break

def process_file(source_path, dest_path):
    """Reads, transforms, and writes file content with error handling"""
    try:
        # Read with explicit encoding
        with open(source_path, 'r', encoding='utf-8') as f:
            content = f.read()
        
        # Apply transformations
        modified_content = transform_content(content)
        
        # Write with explicit encoding
        with open(dest_path, 'w', encoding='utf-8') as f:
            f.write(modified_content)
            
    except IOError as e:
        raise Exception(f"File operation failed: {e}")

def transform_content(content):
    """Example transformations (customize as needed)"""
    # 1. Remove empty lines
    lines = [line.strip() for line in content.splitlines() if line.strip()]
    
    # 2. Number non-empty lines
    numbered_lines = [f"{i+1}. {line}" for i, line in enumerate(lines)]
    
    # 3. Add header/footer
    header = "=== Modified Content ===\n\n"
    footer = f"\n\nTotal lines: {len(numbered_lines)}"
    
    return header + '\n'.join(numbered_lines) + footer

if __name__ == "__main__":
    main()
