```
git clone https://github.com/AssemblyAI-Examples/MinImagen.git
cd MinImagen
python -m venv venv

# Windows
.\venv\Scripts\activate
# macOS/Linux
source venv/bin/activate

pip install minimagen
pip install -r requirements.txt
```

```
python inference.py --CAPTIONS captions.txt --TRAINING_DIRECTORY training_20250131_230128
```

Uses training at specified training directory to generate prompts specified by captions.