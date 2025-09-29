import React, { useState, useMemo } from 'react';
import { Save, X, Loader2, AlertTriangle, UploadCloud } from 'lucide-react';
import { PosterTemplate, PosterStyles, TextStyle, Result } from '../../types';
import { supabase } from '../../lib/supabase';
import DynamicPoster from '../posters/DynamicPoster';
import StyleControl from './StyleControl';
import { defaultPosterStyles } from '../../lib/defaultStyles';

interface PosterEditorProps {
  template: PosterTemplate;
  onClose: () => void;
  onSave: () => void;
}

const PosterEditor: React.FC<PosterEditorProps> = ({ template, onClose, onSave }) => {
  const [isSaving, setIsSaving] = useState(false);
  const [currentLayout, setCurrentLayout] = useState(template.layout_name);
  const [newBackgroundFile, setNewBackgroundFile] = useState<File | null>(null);
  const [backgroundPreview, setBackgroundPreview] = useState<string | null>(template.background_image_url);

  // Deep merge fetched styles with defaults to ensure all properties exist
  const mergedStyles = useMemo(() => {
    const layout = currentLayout || template.layout_name;
    const defaults = defaultPosterStyles[layout] || {};
    const dbStyles = template.styles || {};
    const merged: PosterStyles = {};

    for (const key in defaults) {
      merged[key] = { ...defaults[key], ...(dbStyles[key] || {}) };
    }
    for (const key in dbStyles) {
      if (!merged[key]) {
        merged[key] = dbStyles[key];
      }
    }
    return merged;
  }, [template.styles, currentLayout, template.layout_name]);

  const [styles, setStyles] = useState<PosterStyles>(mergedStyles);

  const handleStyleChange = (element: string, newStyle: Partial<TextStyle>) => {
    setStyles(prev => ({
      ...prev,
      [element]: { ...(prev[element] || {}), ...newStyle } as TextStyle,
    }));
  };
  
  const handleFileChange = (e: React.ChangeEvent<HTMLInputElement>) => {
    if (e.target.files && e.target.files[0]) {
        const file = e.target.files[0];
        setNewBackgroundFile(file);
        const reader = new FileReader();
        reader.onloadend = () => {
            setBackgroundPreview(reader.result as string);
        };
        reader.readAsDataURL(file);
    }
  };

  const handleSave = async () => {
    setIsSaving(true);
    
    // Explicitly define the type for the update payload
    const updateData: {
        styles: PosterStyles;
        layout_name?: string;
        background_image_url?: string;
        thumbnail_url?: string;
    } = {
        styles: styles,
        layout_name: currentLayout,
    };

    try {
        if (newBackgroundFile) {
            const newFilePath = `public/${Date.now()}-${newBackgroundFile.name}`;
            const { error: uploadError } = await supabase.storage
                .from('poster_backgrounds')
                .upload(newFilePath, newBackgroundFile);
            if (uploadError) throw uploadError;

            const { data: { publicUrl } } = supabase.storage
                .from('poster_backgrounds')
                .getPublicUrl(newFilePath);
            
            updateData.background_image_url = publicUrl;
            updateData.thumbnail_url = publicUrl;

            if (template.background_image_url) {
                const oldUrl = template.background_image_url;
                const pathParts = oldUrl.split('/poster_backgrounds/');
                if (pathParts.length > 1) {
                    const oldFilePath = pathParts[1];
                    await supabase.storage.from('poster_backgrounds').remove([oldFilePath]);
                }
            }
        }

        const { error: dbError } = await supabase
            .from('poster_templates')
            .update(updateData)
            .eq('id', template.id);

        if (dbError) throw dbError;

        onSave();
        onClose();
    } catch (err: any) {
        alert(`Error saving template: ${err.message}`);
    } finally {
        setIsSaving(false);
    }
  };
  
  const handleLayoutSelection = (newLayout: string) => {
    setCurrentLayout(newLayout);
    const defaultStylesForLayout = defaultPosterStyles[newLayout] || {};
    const existingStyles = template.styles || {};
    const newMergedStyles: PosterStyles = {};
    for (const key in defaultStylesForLayout) {
      newMergedStyles[key] = { ...defaultStylesForLayout[key], ...(existingStyles[key] || {}) };
    }
    setStyles(newMergedStyles);
  };

  const mockProgram = { event: 'Sample Program', category: 'Sample Category' };
  const mockWinners: Result[] = [
    { id: '1', participant: 'First Winner', school: 'Team A', position: 1, created_at: '', event: '', category: '', year: '' },
    { id: '2', participant: 'Second Winner', school: 'Team B', position: 2, created_at: '', event: '', category: '', year: '' },
    { id: '3', participant: 'Third Winner', school: 'Team C', position: 3, created_at: '', event: '', category: '', year: '' },
  ];

  const layoutOptions = Object.keys(defaultPosterStyles);

  return (
    <div className="fixed inset-0 bg-black/70 z-50 flex items-center justify-center p-4">
      <div className="bg-gray-100 rounded-lg shadow-2xl w-full max-w-7xl h-[95vh] flex flex-col">
        <header className="flex items-center justify-between p-4 border-b bg-white rounded-t-lg">
          <div>
            <h2 className="text-xl font-bold text-gray-800">Style Editor</h2>
            <p className="text-sm text-gray-500">Editing: <span className="font-semibold">{template.name}</span></p>
          </div>
          <div className="flex items-center gap-3">
            <button onClick={handleSave} disabled={isSaving || !currentLayout} className="bg-blue-600 text-white px-4 py-2 rounded-lg flex items-center gap-2 disabled:opacity-50">
              {isSaving ? <Loader2 size={18} className="animate-spin" /> : <Save size={18} />}
              {isSaving ? 'Saving...' : 'Save & Close'}
            </button>
            <button onClick={onClose} className="p-2 rounded-full bg-gray-200 hover:bg-gray-300">
              <X size={20} />
            </button>
          </div>
        </header>

        <div className="flex-grow grid grid-cols-1 lg:grid-cols-3 gap-6 p-6 overflow-hidden">
          {!currentLayout ? (
            <div className="lg:col-span-3 flex flex-col items-center justify-center h-full bg-gray-200 rounded-lg p-8 text-center">
              <AlertTriangle className="w-16 h-16 text-amber-500 mb-4" />
              <h3 className="text-2xl font-bold text-gray-800">Layout Missing</h3>
              <p className="text-gray-600 mb-6 max-w-md">This poster template doesn't have a layout assigned. Please select a base layout to begin editing.</p>
              <select
                onChange={(e) => handleLayoutSelection(e.target.value)}
                defaultValue=""
                className="p-3 border border-gray-300 rounded-lg mb-4 w-full max-w-sm"
              >
                <option value="" disabled>-- Select a Layout --</option>
                {layoutOptions.map(layout => (
                  <option key={layout} value={layout}>{layout.replace(/([A-Z])/g, ' $1').trim()}</option>
                ))}
              </select>
            </div>
          ) : (
            <>
              <div className="lg:col-span-2 bg-gray-800 rounded-lg flex items-center justify-center p-4">
                <div className="w-[500px] h-[500px] shadow-lg">
                  <DynamicPoster
                    template={{...template, layout_name: currentLayout, background_image_url: backgroundPreview}}
                    styles={styles}
                    program={mockProgram}
                    winners={mockWinners}
                    resultNumber={1}
                  />
                </div>
              </div>

              <div className="lg:col-span-1 bg-white rounded-lg p-4 overflow-y-auto space-y-4">
                 {template.layout_name === 'FlexibleLayout' && (
                    <div className="space-y-3 rounded-lg border border-gray-200 p-4 bg-white">
                        <h3 className="font-bold text-lg mb-2">Background</h3>
                        {backgroundPreview && <img src={backgroundPreview} alt="Background Preview" className="w-full h-24 object-cover rounded-md mb-2" />}
                        <input type="file" id="bg-upload" className="hidden" onChange={handleFileChange} accept="image/*" />
                        <label htmlFor="bg-upload" className="cursor-pointer w-full text-center bg-gray-200 text-gray-800 px-4 py-2 rounded-lg text-sm flex items-center justify-center gap-2">
                            <UploadCloud size={16} /> Change Background
                        </label>
                    </div>
                 )}
                <h3 className="font-bold text-lg mb-2">Editable Elements</h3>
                {Object.keys(styles).map((key) => (
                  <StyleControl
                    key={key}
                    label={key}
                    value={styles[key] as TextStyle}
                    onChange={(update) => handleStyleChange(key, update)}
                  />
                ))}
              </div>
            </>
          )}
        </div>
      </div>
    </div>
  );
};

export default PosterEditor;
