# Code Explanation - Eid Design Generator

## HTML Structure

### DOCTYPE & HTML Setup
```html
<!DOCTYPE html>  <!-- Declares this is an HTML5 document -->
<html>           <!-- Root element of the HTML document -->
<head>           <!-- Contains metadata and styling -->
    <title>Custom Design Generator</title>  <!-- Page title shown in browser tab -->
```

---

## CSS Styling (Appearance)

### Universal Reset
```css
* {
    margin: 0;      /* Remove default margins from all elements */
    padding: 0;     /* Remove default padding from all elements */
    box-sizing: border-box;  /* Include padding/border in width/height calculations */
}
```

### Body Styling
```css
body {
    font-family: Arial, sans-serif;  /* Font type for all text */
    background: linear-gradient(135deg, #2c3e50 0%, #4b6584 100%);  /* Gradient background from dark blue to lighter blue */
    min-height: 100vh;      /* Make body at least full viewport height */
    display: flex;          /* Use flexbox layout */
    justify-content: center;  /* Center items horizontally */
    align-items: center;    /* Center items vertically */
    padding: 20px;          /* Add 20px space around edges */
    direction: rtl;         /* Right-to-left text direction (for Arabic) */
    text-align: right;      /* Align text to the right */
}
```

### Container Box
```css
.container {
    background: white;      /* White background box */
    border-radius: 10px;    /* Rounded corners */
    box-shadow: 0 10px 40px rgba(0, 0, 0, 0.2);  /* Drop shadow for depth */
    padding: 40px;          /* 40px space inside the box */
    max-width: 600px;       /* Maximum width of 600px */
    width: 100%;            /* Use full available width (up to max-width) */
}
```

### Design Options Grid
```css
.design-options {
    display: grid;              /* Use CSS grid layout */
    grid-template-columns: 1fr 1fr;  /* Two equal columns */
    gap: 20px;                  /* 20px space between items */
    margin-bottom: 25px;        /* 25px space below */
}

.design-preview {
    width: 100%;        /* Full width of grid cell */
    height: 150px;      /* 150px height */
    border: 3px solid #ddd;    /* 3px gray border */
    border-radius: 8px;        /* Slightly rounded corners */
    background-size: cover;    /* Cover entire area with image */
    background-position: center;  /* Center the image */
}
```

### Radio Button Checked State
```css
.design-option input[type="radio"]:checked + .design-preview {
    border-color: #667eea;      /* Change border to blue when selected */
    transform: scale(1.05);     /* Make it 5% larger */
    box-shadow: 0 5px 15px rgba(102, 126, 234, 0.4);  /* Add blue glow */
}
```

### Design Images
```css
.design1 {
    background-image: url('https://raw.githubusercontent.com/webdesigner-SA/Eid/main/design1.jpg');
    /* Load design1.jpg from GitHub */
}

.design2 {
    background-image: url('https://raw.githubusercontent.com/webdesigner-SA/Eid/main/design2.jpg');
    /* Load design2.jpg from GitHub */
}
```

### Button Styling
```css
.btn-done {
    background: #4b6584;  /* Dark blue background */
    color: white;         /* White text */
}

.btn-done:hover {
    background: #3b5770;      /* Darker blue on hover */
    transform: translateY(-2px);  /* Move up slightly */
}
```

### Preview Section
```css
.preview-section {
    display: none;  /* Hidden by default */
    text-align: center;  /* Center all content */
    padding: 30px;       /* 30px internal space */
}

.preview-section.active {
    display: block;  /* Show when .active class is added */
}

.preview-image {
    width: 100%;      /* Full width */
    max-width: 400px; /* But max 400px */
    height: 300px;    /* 300px height (image preview) */
    position: relative;  /* For positioning child elements */
}
```

---

## JavaScript Functions (Behavior)

### generateDesign() Function
This function runs when user clicks "إنشاء التهنئة" (Create button)

```javascript
function generateDesign() {
    // Get the name user typed in the input field
    const name = document.getElementById('userName').value.trim();
    // Remove extra spaces before/after
    
    // Get which design is selected (design1 or design2)
    const design = document.querySelector('input[name="design"]:checked').value;
    
    // Get the error message element
    const nameError = document.getElementById('nameError');
    
    // Clear any previous error messages
    nameError.textContent = '';
    
    // Check if name is empty
    if (!name) {
        // Show error message
        nameError.textContent = 'Please enter your name';
        // Stop the function (don't continue)
        return;
    }
    
    // Hide the form section
    document.getElementById('formSection').style.display = 'none';
    
    // Show the preview section
    document.getElementById('previewSection').classList.add('active');
    
    // Get the preview image element
    const previewImage = document.getElementById('previewImage');
    
    // Get the preview name element
    const previewName = document.getElementById('previewName');
    
    // Set the design (adds 'design1' or 'design2' class to show right image)
    previewImage.className = 'preview-image ' + design;
    
    // Set the name to display on the image
    previewName.textContent = name;
}
```

### resetForm() Function
This function runs when user clicks "إعادة تعيين" (Reset button)

```javascript
function resetForm() {
    // Clear the name input field
    document.getElementById('userName').value = '';
    
    // Set Design 1 as selected (checked)
    document.getElementById('design1').checked = true;
    
    // Clear any error messages
    document.getElementById('nameError').textContent = '';
}
```

### goBack() Function
This function runs when user clicks "إنشاء تهنئة أخرى" (Create Another button)

```javascript
function goBack() {
    // Remove the 'active' class to hide preview section
    document.getElementById('previewSection').classList.remove('active');
    
    // Show the form section again
    document.getElementById('formSection').style.display = 'block';
    
    // Clear the form (reset inputs)
    resetForm();
}
```

### downloadImage() Function
This function runs when user clicks "تنزيل الصورة" (Download Image button)

```javascript
function downloadImage() {
    // Get the preview image element
    const previewImage = document.getElementById('previewImage');
    
    // Create a canvas (blank drawing surface)
    const canvas = document.createElement('canvas');
    
    // Get the 2D drawing context
    const ctx = canvas.getContext('2d');
    
    // Set canvas size (400 x 300 pixels)
    canvas.width = 400;
    canvas.height = 300;
    
    // Get the user's name from preview
    const name = document.getElementById('previewName').textContent || 'design';
    
    // Check if Design 1 is selected
    const isDesign1 = previewImage.classList.contains('design1');
    
    // Set the image URL based on which design is selected
    const imageUrl = isDesign1 
        ? 'https://raw.githubusercontent.com/webdesigner-SA/Eid/main/design1.jpg'
        : 'https://raw.githubusercontent.com/webdesigner-SA/Eid/main/design2.jpg';
    
    // Create a new Image object
    const img = new Image();
    
    // Allow loading image from different domain (GitHub)
    img.crossOrigin = 'anonymous';
    
    // When image loads successfully
    img.onload = function() {
        // Get image dimensions
        const sw = img.width, sh = img.height;  // Source width/height
        const dw = canvas.width, dh = canvas.height;  // Destination width/height
        
        // Calculate aspect ratios to fit image to canvas properly
        const srcRatio = sw / sh, destRatio = dw / dh;
        
        // Default source position and size
        let sx = 0, sy = 0, sWidth = sw, sHeight = sh;
        
        // If image is wider than canvas aspect ratio
        if (srcRatio > destRatio) {
            // Crop sides of image
            sWidth = sh * destRatio;
            sx = (sw - sWidth) / 2;
        } else {
            // Crop top/bottom of image
            sHeight = sw / destRatio;
            sy = (sh - sHeight) / 2;
        }
        
        // Draw the image on canvas (cropped to fit)
        ctx.drawImage(img, sx, sy, sWidth, sHeight, 0, 0, dw, dh);
        
        // Draw a semi-transparent dark overlay for text readability
        ctx.fillStyle = 'rgba(0, 0, 0, 0.25)';  // 25% opacity black
        ctx.fillRect(0, 0, dw, dh);             // Cover entire canvas
        
        // Draw the name text on the image
        ctx.fillStyle = 'white';           // White text color
        ctx.font = 'bold 48px Arial';      // Font: bold, 48px, Arial
        ctx.textAlign = 'center';          /* Center horizontally */
        ctx.textBaseline = 'middle';       /* Center vertically */
        ctx.shadowColor = 'rgba(0, 0, 0, 0.8)';  // Black shadow
        ctx.shadowBlur = 6;                // Shadow blur amount
        ctx.fillText(name, dw / 2, dh / 2);  // Draw name at center
        
        // Convert canvas to image blob and download
        canvas.toBlob(function(blob) {
            // Create a download URL from the canvas image
            const url = URL.createObjectURL(blob);
            
            // Create a temporary link element
            const a = document.createElement('a');
            a.href = url;                      // Set download link
            a.download = `${name}_design.png`; // Set filename
            a.style.display = 'none';          // Hide the link
            
            // Add link to page (required for some browsers)
            document.body.appendChild(a);
            
            // Trigger the download
            a.click();
            
            // Wait 150ms then clean up
            setTimeout(function() {
                // Remove the link from page
                document.body.removeChild(a);
                
                // Release the download URL from memory
                URL.revokeObjectURL(url);
            }, 150);
        });
    };
    
    // If image fails to load
    img.onerror = function() {
        // Draw a gradient background instead
        const gradient = ctx.createLinearGradient(0, 0, canvas.width, canvas.height);
        
        if (isDesign1) {
            // Design 1 colors
            gradient.addColorStop(0, '#4b6584');
            gradient.addColorStop(1, '#2c3e50');
        } else {
            // Design 2 colors
            gradient.addColorStop(0, '#7d9d9c');
            gradient.addColorStop(1, '#576f72');
        }
        
        // Fill canvas with gradient
        ctx.fillStyle = gradient;
        ctx.fillRect(0, 0, canvas.width, canvas.height);
        
        // Add dark overlay
        ctx.fillStyle = 'rgba(0,0,0,0.25)';
        ctx.fillRect(0, 0, canvas.width, canvas.height);
        
        // Draw the name
        ctx.fillStyle = 'white';
        ctx.font = 'bold 48px Arial';
        ctx.textAlign = 'center';
        ctx.textBaseline = 'middle';
        ctx.fillText(name, canvas.width / 2, canvas.height / 2);
        
        // Download the image (same as above)
        canvas.toBlob(function(blob) {
            const url = URL.createObjectURL(blob);
            const a = document.createElement('a');
            a.href = url;
            a.download = `${name}_design.png`;
            a.style.display = 'none';
            document.body.appendChild(a);
            a.click();
            setTimeout(function() {
                document.body.removeChild(a);
                URL.revokeObjectURL(url);
            }, 150);
        });
    };
    
    // Start loading the image
    img.src = imageUrl;
}
```

---

## HTML Elements (What User Sees)

### Form Section
```html
<div class="form-section active" id="formSection">
    <!-- Title -->
    <h1>صمّم بطاقة تهنئة العيد باسمك</h1>
    <!-- "Design an Eid greeting card with your name" -->
    
    <!-- Name Input -->
    <div class="form-group">
        <label for="userName">الاسم:</label>  <!-- "Name:" -->
        <input type="text" id="userName" placeholder="اكتب اسمك هنا ...">
        <!-- Text input for user's name -->
        <div class="error" id="nameError"></div>
        <!-- Shows error if name is empty -->
    </div>
    
    <!-- Design Selection -->
    <div class="form-group">
        <label>اختر تصميم التهنئة:</label>  <!-- "Choose a design:" -->
        <div class="design-options">
            <!-- Design 1 Option -->
            <div class="design-option">
                <input type="radio" id="design1" name="design" value="design1">
                <!-- Radio button for design 1 -->
                <label for="design1" class="design-preview design1">تصميم ١</label>
                <!-- Design 1 image preview -->
            </div>
            
            <!-- Design 2 Option -->
            <div class="design-option">
                <input type="radio" id="design2" name="design" value="design2">
                <!-- Radio button for design 2 -->
                <label for="design2" class="design-preview design2">تصميم ٢</label>
                <!-- Design 2 image preview -->
            </div>
        </div>
    </div>
    
    <!-- Buttons -->
    <div class="button-group">
        <button class="btn-done" onclick="generateDesign()">إنشاء التهنئة</button>
        <!-- "Create" button - calls generateDesign() -->
        <button class="btn-reset" onclick="resetForm()">إعادة تعيين</button>
        <!-- "Reset" button - calls resetForm() -->
    </div>
</div>
```

### Preview Section
```html
<div class="preview-section" id="previewSection">
    <!-- Hidden by default, shown when user clicks Create -->
    
    <h1>تهنئتك جاهزة</h1>
    <!-- "Your greeting is ready" -->
    
    <!-- Image Preview -->
    <div class="preview-image" id="previewImage">
        <!-- This div shows the selected design image -->
        <div class="preview-name" id="previewName"></div>
        <!-- This div contains the user's name overlaid on image -->
    </div>
    
    <!-- Download Button -->
    <button class="btn-download" onclick="downloadImage()">تنزيل الصورة</button>
    <!-- "Download Image" button - calls downloadImage() -->
    
    <!-- Back Button -->
    <button class="btn-back" onclick="goBack()">إنشاء تهنئة أخرى</button>
    <!-- "Create Another" button - calls goBack() -->
</div>
```

---

## How Everything Works Together

1. **User opens website** → Form section displays with name input and design choices

2. **User enters name and selects design** → Form is filled

3. **User clicks "إنشاء التهنئة"** → `generateDesign()` runs:
   - Gets name and design choice
   - Checks if name is empty
   - Hides form, shows preview
   - Sets the selected design image
   - Displays name on preview

4. **User clicks "تنزيل الصورة"** → `downloadImage()` runs:
   - Creates a canvas
   - Loads the design image
   - Draws image on canvas
   - Adds dark overlay
   - Draws white text with name
   - Downloads as PNG file

5. **User clicks "إنشاء تهنئة أخرى"** → `goBack()` runs:
   - Hides preview, shows form
   - Clears all inputs
   - Ready for next design

---

## Key Concepts

- **CSS**: Handles all styling (colors, layout, fonts, animations)
- **HTML**: The structure and content users see
- **JavaScript**: Makes things interactive (responding to button clicks)
- **Canvas API**: Used to draw and merge images with text
- **GitHub URLs**: Images are loaded from GitHub so they work anywhere online
