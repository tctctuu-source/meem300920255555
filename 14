import React, { useState, useRef, useEffect } from 'react';
import { motion, AnimatePresence } from 'framer-motion';
import { X, Download, Share2, ArrowLeft, ArrowRight, Loader2 } from 'lucide-react';
import { toPng } from 'html-to-image';
import { supabase } from '../lib/supabase';
import { Result, PosterTemplate } from '../types';
import DynamicPoster from './posters/DynamicPoster';

interface PosterModalProps {
  isOpen: boolean;
  onClose: () => void;
  data: {
    program: { event: string; category: string };
    winners: Result[];
    resultNumber: number;
  } | null;
}

const PosterModal: React.FC<PosterModalProps> = ({ isOpen, onClose, data }) => {
  const [activeTemplates, setActiveTemplates] = useState<PosterTemplate[]>([]);
  const [loadingPosters, setLoadingPosters] = useState(true);
  const [activeStyle, setActiveStyle] = useState(0);
  const [isDownloading, setIsDownloading] = useState(false);
  const posterRef = useRef<HTMLDivElement>(null);

  useEffect(() => {
    if (!isOpen) return;

    const fetchAndSetPosters = async () => {
      setLoadingPosters(true);
      try {
        const { data: templates, error } = await supabase
          .from('poster_templates')
          .select('*')
          .eq('is_active', true)
          .order('created_at');

        if (error) throw error;
        
        const validTemplates = templates.filter(t => t.styles && t.layout_name);
        setActiveTemplates(validTemplates as PosterTemplate[]);

      } catch (err) {
        console.error("Error fetching poster templates:", err);
        setActiveTemplates([]);
      } finally {
        setLoadingPosters(false);
        setActiveStyle(0);
      }
    };

    fetchAndSetPosters();
  }, [isOpen]);

  if (!isOpen || !data) return null;

  const handleDownload = async () => {
    if (!posterRef.current) return;
    setIsDownloading(true);
    try {
      const dataUrl = await toPng(posterRef.current, { cacheBust: true, quality: 1.0, pixelRatio: 2 });
      const link = document.createElement('a');
      link.download = `muhimmath-result-${data.program.event.toLowerCase().replace(/\s/g, '-')}.png`;
      link.href = dataUrl;
      link.click();
    } catch (err) {
      console.error('Failed to download image', err);
      alert('Sorry, there was an error downloading the poster.');
    } finally {
      setIsDownloading(false);
    }
  };

  const handleShare = async () => {
    const shareData = {
      title: `Result: ${data.program.event}`,
      text: `Check out the results for ${data.program.event} - ${data.program.category} from Muhimmath!`,
      url: window.location.href,
    };
    try {
      if (navigator.share) {
        await navigator.share(shareData);
      } else {
        await navigator.clipboard.writeText(window.location.href);
        alert('Link copied to clipboard!');
      }
    } catch (err) {
      console.error('Failed to share', err);
      alert('Sharing failed. You can copy the link from the address bar.');
    }
  };

  const nextStyle = () => setActiveStyle((prev) => (prev + 1) % activeTemplates.length);
  const prevStyle = () => setActiveStyle((prev) => (prev - 1 + activeTemplates.length) % activeTemplates.length);

  const activeTemplate = activeTemplates.length > 0 ? activeTemplates[activeStyle] : null;

  return (
    <AnimatePresence>
      {isOpen && (
        <motion.div
          initial={{ opacity: 0 }}
          animate={{ opacity: 1 }}
          exit={{ opacity: 0 }}
          className="fixed inset-0 bg-black bg-opacity-80 z-50 flex items-center justify-center p-4"
          onClick={onClose}
        >
          <motion.div
            initial={{ scale: 0.9, y: 20 }}
            animate={{ scale: 1, y: 0 }}
            exit={{ scale: 0.9, y: 20 }}
            className="bg-brand-dark-blue rounded-2xl w-full max-w-5xl h-full max-h-[95vh] flex flex-col lg:flex-row overflow-hidden"
            onClick={(e) => e.stopPropagation()}
          >
            {/* Poster Preview */}
            <div className="flex-grow bg-brand-dark-blue/50 flex items-center justify-center p-4 lg:p-8 overflow-auto">
              {loadingPosters ? (
                <Loader2 className="w-10 h-10 text-white animate-spin" />
              ) : activeTemplate && activeTemplate.styles ? (
                <div ref={posterRef} className="w-[400px] h-[400px] lg:w-[500px] lg:h-[500px] shadow-2xl">
                  <DynamicPoster
                    template={activeTemplate}
                    styles={activeTemplate.styles}
                    program={data.program}
                    winners={data.winners}
                    resultNumber={data.resultNumber}
                  />
                </div>
              ) : (
                <div className="text-white text-center">
                  <p>No active poster designs found.</p>
                  <p className="text-sm text-white/70">Please activate and configure a design in the admin panel.</p>
                </div>
              )}
            </div>

            {/* Controls */}
            <div className="w-full lg:w-72 bg-brand-dark-blue border-t lg:border-t-0 lg:border-l border-brand-mid-blue/50 p-6 flex flex-col justify-between">
              <div>
                <h3 className="text-ui-text-light text-xl font-bold mb-1">Result Poster</h3>
                <p className="text-ui-text-light/70 text-sm mb-6">Download or share this result</p>

                <div className="space-y-4">
                  <button
                    onClick={handleDownload}
                    disabled={isDownloading || loadingPosters || !activeTemplate}
                    className="w-full bg-brand-coral text-brand-dark-blue font-bold py-3 px-4 rounded-lg flex items-center justify-center gap-2 hover:bg-opacity-90 transition-colors disabled:opacity-50"
                  >
                    {isDownloading ? <Loader2 className="w-5 h-5 animate-spin" /> : <Download className="w-5 h-5" />}
                    {isDownloading ? 'Downloading...' : 'Download Poster'}
                  </button>
                  <button
                    onClick={handleShare}
                    disabled={loadingPosters || !activeTemplate}
                    className="w-full bg-brand-mid-blue text-ui-text-light font-bold py-3 px-4 rounded-lg flex items-center justify-center gap-2 hover:bg-brand-mid-blue/80 transition-colors disabled:opacity-50"
                  >
                    <Share2 className="w-5 h-5" />
                    Share Result
                  </button>
                </div>
              </div>

              <div>
                <p className="text-ui-text-light/70 text-sm text-center mb-2">Change Style</p>
                <div className="flex items-center justify-between">
                  <button onClick={prevStyle} disabled={loadingPosters || activeTemplates.length <= 1} className="p-3 bg-brand-mid-blue/50 rounded-full hover:bg-brand-mid-blue text-ui-text-light disabled:opacity-50"><ArrowLeft size={20} /></button>
                  <span className="text-ui-text-light font-mono">{loadingPosters ? '-/-' : `${activeStyle + 1} / ${activeTemplates.length}`}</span>
                  <button onClick={nextStyle} disabled={loadingPosters || activeTemplates.length <= 1} className="p-3 bg-brand-mid-blue/50 rounded-full hover:bg-brand-mid-blue text-ui-text-light disabled:opacity-50"><ArrowRight size={20} /></button>
                </div>
              </div>
            </div>
            <button onClick={onClose} className="absolute top-4 right-4 text-ui-text-light bg-black bg-opacity-50 p-2 rounded-full hover:bg-opacity-75 z-10"><X size={24} /></button>
          </motion.div>
        </motion.div>
      )}
    </AnimatePresence>
  );
};

export default PosterModal;
